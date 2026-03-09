# Workshop 03: Custom Agents

**Duration: ~20 minutes**

## What You'll Learn

- What subagents are and how they differ from the main session
- How to define custom agents in `.claude/agents/`
- Agent configuration: tools, model, permissions, hooks
- Two ways to invoke agents: deterministic (`@agent-name`) vs probabilistic (natural language)

---

## What Are Subagents?

Subagents are **isolated workers** with their own context window. The main Claude session delegates a task, the subagent works on it independently, and returns a result.

```
┌─────────────────────────────────────────────────┐
│              Main Claude Session                 │
│                                                  │
│  "Run the tests and review the cart module"      │
│                                                  │
│       ┌──────────┐        ┌──────────┐          │
│       │  Agent:  │        │  Agent:  │          │
│       │  test-   │        │  reviewer│          │
│       │  runner  │        │          │          │
│       ├──────────┤        ├──────────┤          │
│       │Own context│       │Own context│          │
│       │Own tools  │       │Own tools  │          │
│       │Own limits │       │Own limits │          │
│       └─────┬────┘        └─────┬────┘          │
│             │                   │                │
│             ▼                   ▼                │
│         "All 24 tests       "Found 2            │
│          passing"            issues in            │
│                              CartContext"        │
└─────────────────────────────────────────────────┘
```

**Key properties:**
- **Separate context window** — doesn't consume the main session's context
- **Scoped tools** — you control what each agent can access
- **Focused task** — agent works on one thing, returns a result
- **Parallel capable** — multiple agents can run simultaneously

---

## Built-in Agents

Claude Code comes with several built-in agents:

| Agent | Model | Tools | Purpose |
|-------|-------|-------|---------|
| `Explore` | Haiku | Read-only | Fast codebase exploration |
| `Plan` | Inherits | Read-only | Research for planning |
| `general-purpose` | Inherits | All | Complex multi-step tasks |

You've already used these — when Claude says "Let me explore the codebase," it's spawning the `Explore` agent.

---

## Creating Custom Agents

Custom agents are markdown files with YAML frontmatter. They live in:

| Location | Path | Scope |
|----------|------|-------|
| Project | `.claude/agents/<name>.md` | Shared via git |
| Personal | `~/.claude/agents/<name>.md` | All your projects |

### Agent File Structure

```markdown
---
name: agent-name
description: What this agent does. When to use it.
tools: Read, Grep, Glob, Bash
model: sonnet
maxTurns: 10
---

You are an expert at [specific task].

When invoked:
1. Do this first
2. Then do this
3. Report results

## Guidelines
- Be thorough
- Follow project conventions
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique identifier (lowercase, hyphens) |
| `description` | Yes | When Claude should delegate to this agent |
| `tools` | No | Allowed tools: `Read`, `Grep`, `Glob`, `Bash`, `Edit`, `Write`, etc. |
| `disallowedTools` | No | Tools to explicitly deny |
| `model` | No | `haiku`, `sonnet`, `opus`, or `inherit` (default) |
| `maxTurns` | No | Max agentic turns before stopping |
| `permissionMode` | No | `default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions` |
| `isolation` | No | Set to `worktree` for isolated git worktree |
| `background` | No | `true` to always run as background task |
| `hooks` | No | Agent-scoped lifecycle hooks |

---

## Task 1: Create a Test Runner Agent

This agent runs the Tea Store's full test suite — frontend (Vitest), backend (Pytest), and e2e (Playwright) — and reports results.

Create the agents directory:

```bash
mkdir -p .claude/agents
```

Create `.claude/agents/test-runner.md`:

```markdown
---
name: test-runner
description: Runs all Tea Store tests (frontend, backend, e2e) and reports results. Use when tests need to be run or verified.
tools: Read, Bash, Glob, Grep
model: sonnet
maxTurns: 15
---

You are a test runner for the Tea Store project. Run the test suites and report results clearly.

## Project Test Structure

The Tea Store has three test layers:

- **Frontend unit tests**: Vitest + Testing Library in `frontend/src/`
  - Run with: `cd frontend && npm test`
- **Backend unit tests**: Pytest in `backend/tests/`
  - Run with: `cd backend && source venv/bin/activate && pytest`
- **E2E tests**: Playwright in `e2e/`
  - Run with: `npx playwright test`
  - Requires both frontend (port 4321) and backend (port 8765) running

## Steps

1. **Read `package.json` files** to confirm available test scripts.

2. **Run frontend tests**: `cd frontend && npm test`

3. **Run backend tests**: `cd backend && source venv/bin/activate && pytest -v`

4. **Run e2e tests** (only if both servers are running): `npx playwright test`
   - If servers aren't running, skip e2e and note it in the report.

5. **Report results** in a structured summary.

## Output Format

Always end with a summary table:

| Suite | Passed | Failed | Skipped |
|-------|--------|--------|---------|
| Frontend (Vitest) | X | X | X |
| Backend (Pytest)  | X | X | X |
| E2E (Playwright)  | X | X | X |

If any tests fail, include the failure details with file paths and error messages.

## Rules

- Never modify test files or source code
- Run tests in order: frontend unit → backend unit → e2e
- If a test suite can't run (missing dependencies, servers not running), skip it and note why
- Always report the full output for failures
```

---

## Task 2: Check Your Agents with `/agents`

After creating the agent file, verify it's registered. Start a new Claude Code session and run:

```
/agents
```

You should see a list that includes:

- Built-in agents (`Explore`, `Plan`, `general-purpose`)
- Your custom `test-runner` agent with its description

The `/agents` menu also lets you:
- **Create** new agents with guided setup
- **Edit** existing agent configuration
- **View** which tools each agent has access to
- **See priority** when agents with the same name exist at different scopes

---

## Two Ways to Invoke Agents

This is the most important concept for using agents effectively. There are two invocation patterns:

```
┌────────────────────────────────────────────────────────────┐
│                                                            │
│  Deterministic (you choose)      Probabilistic (AI picks) │
│  ─────────────────────────       ─────────────────────────│
│                                                            │
│  @test-runner run all tests      run the tests and report │
│  │                                │                        │
│  ▼                                ▼                        │
│  Guaranteed to invoke            Claude reads the agent's  │
│  test-runner agent               description and decides   │
│                                  whether to delegate       │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### Deterministic: `@agent-name`

Type `@` followed by the agent name to **guarantee** that agent is invoked:

```
@test-runner run all tests and report results
```

```
@reviewer check the CartContext for issues
```

This is like calling a function by name — you know exactly which agent will handle it. Use this when:
- You want a **specific** agent, no ambiguity
- You're demonstrating or testing an agent
- The task clearly belongs to one agent

### Probabilistic: Natural Language

Describe what you want, and Claude decides which agent (if any) to delegate to:

```
run the tests and give me a report
```

```
check my recent changes for quality issues
```

Claude reads the `description` field of each available agent and matches your request. This is more natural but less predictable — Claude might:
- Pick the right agent
- Pick a different agent
- Handle the task itself without delegating

The `description` field is your control lever here. A clear, specific description leads to better matching:

```yaml
# Vague — Claude may or may not match this
description: Helps with testing

# Specific — Claude knows exactly when to use this
description: Runs all Tea Store tests (frontend, backend, e2e) and reports results. Use when tests need to be run or verified.
```

---

## Task 3: Test Both Invocation Styles

Start a new Claude Code session.

### Deterministic test:

```
@test-runner run the frontend tests only
```

The test-runner agent should be invoked directly. You'll see it working in its own context.

### Probabilistic test:

```
I just changed CartContext.tsx — can you make sure nothing is broken?
```

Claude should recognize this as a testing task and delegate to the test-runner agent based on its description. Watch whether Claude delegates or handles it directly — this is the probabilistic nature at work.

---

## Task 4: Create a Code Reviewer Agent

Create `.claude/agents/reviewer.md`:

```markdown
---
name: reviewer
description: Reviews code changes for quality, security, and convention compliance. Use after implementing features or fixes.
tools: Read, Grep, Glob, Bash
model: sonnet
maxTurns: 15
---

You are a senior code reviewer for the Tea Store project.

## Project Context

- **Frontend**: React 19 + TypeScript + Vite, components in `frontend/src/components/`
- **Backend**: FastAPI (Python), routes in `backend/app/api/routes.py`
- **Tests**: Vitest (frontend), Pytest (backend), Playwright (e2e)
- **State management**: React Context (`frontend/src/contexts/`)
- **API services**: `frontend/src/services/`

## Steps

1. **Get the diff**: Run `git diff` to see what changed. If on a branch, also run `git diff main...HEAD`.

2. **Read changed files**: Read each modified file in full to understand context.

3. **Read project conventions**: Check `CLAUDE.md` and any linting/formatting configs.

4. **Review** against this checklist:

### Review Checklist

- [ ] **Correctness**: Does the code do what it's supposed to?
- [ ] **Error handling**: Are edge cases handled? (null products, empty cart, API failures)
- [ ] **Security**: No hardcoded secrets, XSS via dangerouslySetInnerHTML, or unvalidated inputs?
- [ ] **Types**: Are TypeScript types properly used? (check `frontend/src/types/`)
- [ ] **State**: Are Context updates correct? Does localStorage sync work?
- [ ] **Naming**: Are variables and functions clearly named?
- [ ] **Tests**: Are new behaviors covered by tests?

## Output Format

For each issue found:
- **File**: path/to/file.ts:42
- **Severity**: Critical / Warning / Suggestion
- **Issue**: What's wrong
- **Fix**: How to fix it

End with an overall assessment: APPROVE, REQUEST CHANGES, or NEEDS DISCUSSION.

## Rules

- Never modify any files — this is a read-only review
- Be specific — cite line numbers and code snippets
- Don't nitpick formatting if a linter handles it
- Focus on issues that matter: bugs, security, maintainability
```

### Verify It Works

Make a small change to a file, then invoke the reviewer deterministically:

```
@reviewer check my recent changes
```

The agent should produce a structured review with file references and severity levels.

Then verify `/agents` shows both agents:

```
/agents
```

You should now see `test-runner` and `reviewer` listed alongside the built-in agents.

---

## How Agents Get Invoked — Summary

| Method | Syntax | Behavior |
|--------|--------|----------|
| **`@agent-name`** | `@test-runner run tests` | Deterministic — always invokes that agent |
| **Natural language** | `run the tests` | Probabilistic — Claude matches to an agent's description |
| **`/agents` menu** | `/agents` → select | Interactive — browse and invoke from a list |
| **CLI flag** | `claude --agent test-runner` | Starts the session as that agent |

**When to use which:**

- **`@name`** when you know exactly which agent you want
- **Natural language** when you want Claude to pick the best approach
- **`/agents`** when you need to browse or manage agents
- **`--agent`** when you want the entire session to be that agent

---

## Agent Design Principles

### Keep Agents Focused

Each agent should do **one thing well**. Don't create a "do-everything" agent.

```
Good:
  test-runner — runs tests, reports results
  reviewer — reviews code, reports issues
  migrator — handles database migrations

Bad:
  helper — runs tests, reviews code, writes docs, deploys
```

### Scope Tools Tightly

Give agents only the tools they need:

```yaml
# Read-only agent — can't modify anything
tools: Read, Grep, Glob

# Test agent — can run commands but not edit files
tools: Read, Grep, Glob, Bash

# Full agent — can read, write, and execute
tools: Read, Grep, Glob, Bash, Edit, Write
```

### Use Cheaper Models for Simple Tasks

```yaml
# Exploration doesn't need the strongest model
model: haiku

# Code review benefits from deeper reasoning
model: sonnet

# Complex multi-step tasks
model: opus
```

### Write Good Descriptions

The `description` is what Claude uses for probabilistic matching. Be specific about **what** the agent does and **when** to use it:

```yaml
# Weak — too vague for reliable matching
description: Helps with code

# Strong — Claude knows exactly when to delegate
description: Runs all Tea Store tests (frontend, backend, e2e) and reports
  results. Use when tests need to be run or verified.
```

---

## Agents vs Skills

Both automate workflows, but differently:

| | Skills | Agents |
|---|--------|--------|
| **What they are** | Markdown instructions loaded into context | Isolated workers with own context |
| **Context** | Shares main session context | Has its own context window |
| **Invoked by** | You (via `/skill-name`) | You (`@name`) or Claude (probabilistic) |
| **Best for** | Multi-step workflows (commit, fix-issue) | Parallel/isolated tasks (test, review) |
| **From Workshop** | Workshop 1 | Workshop 2 |

**Rule of thumb:** If the task needs to see the full conversation context, use a skill. If it's an independent task, use an agent.

---

## Key Takeaways

- **Agents are isolated workers** with their own context window and scoped tools
- **`@agent-name`** for deterministic invocation, **natural language** for probabilistic
- **`/agents`** to list, create, edit, and manage all available agents
- The **`description` field** controls probabilistic matching — make it specific
- Put agents in **`.claude/agents/`** for project-shared, **`~/.claude/agents/`** for personal
- **Scope tools tightly** — give agents only what they need
- Use **cheaper models** (`haiku`, `sonnet`) for simpler agent tasks

---

Continue to: [04_parallel_workflows.md](./04_parallel_workflows.md)
