# Module 5: Autonomous Loops — Ralph & LISA

**Duration: ~30 minutes**
**Format:** Presentation + Hands-on Demo

---

## What You'll Learn

- What the Ralph Loop is and how it enables autonomous, iterative development
- Why context bloating breaks long-running agents and how state files solve it
- What LISA is and how it generates structured specs that Ralph can execute
- How to install the Ralph and LISA plugins and run them on the Tea Store project

## Prerequisites

- Completed [Module 2: Key Components](./02_key_components.md) — skills, agents, and command created
- Completed [Module 4: Plugins & Marketplace](./04_plugins_marketplace_architecture.md) — understand plugin installation
- Tea Store project running locally

---

## 1. The Problem: Agents That Stop Too Soon (5 min)

In Module 2 you built agents that run in isolation and return a result. But each agent runs **once** — it does its job, returns output, and exits. What happens when the job isn't done in one pass?

Real-world scenarios where a single pass isn't enough:

| Scenario | Why one pass fails |
|----------|-------------------|
| Tests fail after implementation | Agent exits before fixing |
| Code review finds issues | No automatic retry loop |
| Build breaks mid-implementation | Agent doesn't see the error and try again |
| Large feature with many subtasks | Context window fills up before finishing |

You could manually re-run the agent, feed it the error output, and ask it to try again. But that defeats the purpose of automation.

### What If the Agent Could Loop?

Imagine an agent that:
1. Works on a task
2. Checks if the task is truly done (tests pass, criteria met)
3. If not done — loops back and tries again, seeing its own previous work
4. Repeats until the task is genuinely complete

This is the **Ralph Loop**.

---

## 2. What Is the Ralph Loop? (5 min)

The Ralph Loop is a technique where Claude Code runs in a continuous loop, receiving the **same prompt** each iteration, but seeing the **accumulated results** of its previous work in the project files.

Named after Ralph Wiggum from The Simpsons, the philosophy is simple: **persistent iteration wins**.

### How It Works

```
┌──────────────────────────────────────────────────────┐
│                   RALPH LOOP                         │
│                                                      │
│   ┌─────────┐     ┌──────────────┐     ┌──────────┐  │
│   │  PROMPT │────►│  Claude Code │────►│  WORK    │  │
│   │ (same   │     │  works on    │     │ (files,  │  │ 
│   │  every  │     │  the task    │     │  tests,  │  │
│   │  time)  │     │              │     │  code)   │  │
│   └─────────┘     └──────┬───────┘     └──────────┘  │
│        ▲                 │                           │
│        │                 ▼                           │
│        │          ┌──────────────┐                   │
│        │          │ Try to exit  │                   │
│        │          └──────┬───────┘                   │
│        │                 │                           │
│        │          ┌──────▼───────┐                   │
│        │          │ Stop Hook    │                   │
│        │          │ checks:      │                   │
│        │          │ Done?        │                   │
│        │          └──┬───────┬───┘                   │
│        │          NO │       │ YES                   │
│        │             │       │                       │
│        └─────────────┘       ▼                       │
│                        EXIT (done)                   │
│                                                      │
└──────────────────────────────────────────────────────┘
```

The key mechanism is a **Stop Hook** — a Claude Code hook that intercepts exit attempts. When Claude tries to exit:

- **If a completion promise is detected** → allow exit, the task is done
- **If not** → block exit, feed the same prompt back, start next iteration

Each iteration, Claude sees:
- The same prompt (what to do)
- Modified project files (its own previous work)
- Git history (what changed)
- Test results (what's passing and failing)

This creates a **self-correcting feedback loop** where Claude iteratively improves until the task is genuinely complete.

### Completion Promises

How does the loop know when to stop? Claude outputs a **completion promise** — a special tag that signals the task is done:

```
<promise>COMPLETE</promise>
```

The Stop Hook reads Claude's output. If it finds the promise tag matching the configured text, it allows the exit. Otherwise, it blocks and loops.

This prevents Claude from exiting prematurely — it can only escape the loop by declaring (truthfully) that the work is done.

---

## 3. The Context Problem and LISA (5 min)

### Why Loops Break: Context Bloating

The Ralph Loop solves the "stops too soon" problem. But it introduces a new one: **context bloating**.

Each iteration adds to the context window — files read, commands run, reasoning, output. After several iterations, Claude's context fills up and it loses track of what it's doing.

```
Iteration 1:  [prompt][work][work][work].............. ← plenty of room
Iteration 5:  [prompt][work][work][work][work][work].. ← getting full
Iteration 10: [prompt][work][work][work][work][work][w ← context limit
```

### The Solution: Start Fresh, Read State

Instead of accumulating context across iterations, each iteration can **start with a clean context** and read a **state file** to understand what's already been done.

This is where **LISA** comes in.

### What Is LISA?

LISA (Lisa Interview Specification Architecture) is a planning tool that conducts an interactive interview about your feature and generates a **structured specification** — a detailed PRD in both Markdown and JSON format.

The key insight: LISA produces specs in a format that Ralph can **read at the start of each iteration** to know exactly what to work on next.

### Lisa Plans. Ralph Does.

```
┌──────────────────┐          ┌──────────────────┐
│      LISA        │          │      RALPH       │
│                  │          │                  │
│  Interview you   │          │  Read the spec   │
│  about the       │  ──────► │  Loop until      │
│  feature         │  spec    │  all criteria    │
│                  │  files   │  are met         │
│  Output:         │          │                  │
│  • PRD (.md)     │          │  Each iteration: │
│  • Spec (.json)  │          │  • Read spec     │
│  • Progress file │          │  • Check status  │
│                  │          │  • Work on next  │
│                  │          │    incomplete    │
│                  │          │    item          │
└──────────────────┘          └──────────────────┘
```

The **progress file** acts as persistent state. Ralph reads it at the start of each iteration to know what's done and what's remaining — even if the context window was cleared between iterations.

---

## 4. Install the Plugins (5 min)

Both Ralph and LISA are available as Claude Code plugins. Install them into the Tea Store project.

### Install the Ralph Plugin

In your Claude Code session:

```
/plugin add marketplace https://github.com/anthropics/claude-code
/plugin add ralph-wiggum
```

Restart Claude Code to load the plugin:

```bash
/exit
claude
```

Verify the Ralph commands are available:

```
/ralph-loop --help
```

You should see options for `--max-iterations` and `--completion-promise`.

### Install the LISA Plugin

```
/plugin add marketplace https://github.com/blencorp/lisa
/plugin add lisa
```

Restart Claude Code:

```bash
/exit
claude
```

Verify the LISA commands are available:

```
/lisa:help
```

---

## 5. Run LISA → Ralph on the Tea Store (10 min)

Let's run the full plan-then-execute workflow on a real feature.

### Step 1: Plan with LISA

Start a LISA interview for a new feature:

```
/lisa:plan "customer-login"
```

LISA will ask you a series of questions about the feature. Answer them conversationally. For example:

- **What does the login feature do?** Customers can register with email and password, log in, and see their account details.
- **How is it stored?** Backend API endpoints for register, login, and session management. Passwords hashed and stored in the database.
- **What UI?** A login/register page with email and password fields. A "Log in" button in the header that changes to the user's name when logged in.
- **Edge cases?** Invalid credentials show an error. Duplicate email registration is rejected. Sessions expire after inactivity.
- **Testing?** Backend API tests with Pytest, frontend component tests with Vitest.

When you've answered enough questions, type:

```
done
```

LISA generates three files in your project:

```
specs/customer-login.md          ← Human-readable PRD
specs/customer-login.json        ← Machine-readable spec
specs/customer-login-progress.txt ← Progress tracker for Ralph
```

### Step 2: Execute with Ralph

Now hand the spec to Ralph. Start the loop:

```
/ralph-loop:ralph-loop "Implement the feature described in specs/customer-login.md.
Read specs/customer-login-progress.txt at the start of each iteration
to check what's done. For each incomplete user story, implement it using
TDD (write tests first, then code). After implementing each story, run
the tests with 'node test.js'. Update the progress file after completing
each story. When ALL stories pass their acceptance criteria and all tests
pass, output <promise>COMPLETE</promise>." --completion-promise "COMPLETE" --max-iterations 20
```

### What Happens

Watch the loop run:

```
Iteration 1:  Reads spec → implements user registration API endpoint → runs tests → updates progress
Iteration 2:  Reads progress → sees register done → implements login endpoint → tests → updates
Iteration 3:  Reads progress → fixes failing test for invalid credentials → passes → updates
Iteration 4:  Reads progress → implements login/register UI page → runs tests → updates
Iteration 5:  Reads progress → implements header login button and session display → tests → updates
...
Iteration N:  Reads progress → all stories done → all tests pass → <promise>COMPLETE</promise>
              → Loop exits
```

Each iteration:
1. Reads the progress file to know what's been done
2. Finds the next incomplete story from the spec
3. Implements it using TDD
4. Runs tests to verify
5. Updates the progress file
6. Tries to exit → Stop Hook checks for promise → loops if not done

### Cancel If Needed

If you need to stop the loop early:

```
/cancel-ralph
```

---

## 6. How Ralph Prevents Context Bloating

The progress file is the key to keeping the loop running across many iterations without context overload.

### Without State (context bloats)

```
Iteration 1: [read spec][implement story 1][run tests][fix bug]
Iteration 2: [all of iteration 1 still in context][implement story 2]
Iteration 3: [iterations 1+2 in context][running out of room...]
```

### With State File (context stays clean)

```
Iteration 1: [read spec][read progress: nothing done][implement story 1][update progress]
Iteration 2: [read spec][read progress: story 1 done][implement story 2][update progress]
Iteration 3: [read spec][read progress: 1,2 done][implement story 3][update progress]
```

The progress file acts as **external memory**. Each iteration starts by reading the file to reconstruct what's been done, rather than relying on accumulated context from previous iterations.

This is the same principle behind how databases work — you don't keep everything in memory, you persist state externally and query it when needed.

---

## Key Takeaways

| Concept | What It Does |
|---------|-------------|
| **Ralph Loop** | Runs Claude in a continuous loop with the same prompt until a completion promise is detected |
| **Stop Hook** | Intercepts exit attempts, blocks them if work isn't done, feeds the prompt back |
| **Completion Promise** | `<promise>TEXT</promise>` tag that signals the task is genuinely complete |
| **LISA** | Conducts a feature interview and generates a structured spec (PRD + JSON + progress file) |
| **Progress File** | External state that Ralph reads each iteration to know what's done — prevents context bloating |

### When to Use Ralph

| Good For | Not Good For |
|----------|-------------|
| Well-defined tasks with clear completion criteria | Tasks requiring human judgment mid-process |
| Iterative implementation (TDD, build-test-fix) | One-shot operations (create a file, run a query) |
| Tasks with automatic verification (test suites) | Unclear or subjective success criteria |
| Multi-story features with a structured spec | Production debugging with unknown scope |

### The Full Pipeline

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│   LISA   │────►│  RALPH   │────►│  Tests   │────►│  Done    │
│          │spec │          │code │  pass?   │ yes │          │
│ Interview│     │ Loop     │     │          │     │ Promise  │
│ → PRD    │     │ → TDD    │     │ no → loop│     │ → exit   │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
```

---

## Reference

- **Ralph Wiggum Plugin:** https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum
- **LISA Plugin:** https://github.com/blencorp/lisa
- **Original Ralph Technique:** https://ghuntley.com/ralph/

---

## Next Up

Continue to: [06_context_engineering_how.md](./06_context_engineering_how.md) — Map Your Team's Context Gaps
