# Workshop 07: Introduction to Agents

**Duration: ~15 minutes**

## What You'll Learn

- Why skills in the main context can become problematic
- What agents are and how they differ from skills
- How agents run in isolated context windows
- How to inject skills into agents for reusable knowledge
- Update `/deploy-ticket` to use an agent

---

## The Context Problem

In the previous modules, you built four skills that run in the **main context window**. Every file Claude reads, every command it runs, and every response it generates — all of that accumulates in the shared context.

Let's see this in action.

**Step 1: Check your current context**

In Claude Code, type:

```
/context
```

Note the current context usage (e.g., "Context: 12,450 tokens").

**Step 2: Run the deploy-ticket command**

```
/deploy-ticket Add a product rating feature to PROJECT_KEY
```

(Replace PROJECT_KEY with your Jira project key)

**Step 3: Check context again**

```
/context
```

Notice how the context has grown significantly. Every file read, every Jira API call, every piece of reasoning — it's all in your context now.

**The problem:** If you run multiple features through `/deploy-ticket`, your context fills up. Claude may lose track of earlier information, and you'll eventually need to start fresh.

---

## Skills vs Agents

| | Skills | Agents |
|---|--------|--------|
| **What they are** | Knowledge files | Isolated executors |
| **Where they run** | Main context window | Their own context window |
| **Format** | `SKILL.md` | `AGENT.md` |
| **Location** | `.claude/skills/` | `.claude/agents/` |
| **Purpose** | Teach *how* to do something | *Do* something focused |
| **Context impact** | Adds to main context | Only result returns to main |

**Key insight:** Skills carry knowledge. Agents carry intent and execute in isolation.

---

## How Agent Isolation Works

When an agent runs:

```
┌─────────────────────────────────────────────┐
│          Main Context Window                │
│                                             │
│  You: "Run /deploy-ticket for feature X"   │
│                                             │
│  ┌──────────────────┐   Result only         │
│  │ implementer agent │ ──────────►  Summary │
│  │  (own context)    │   returned to main   │
│  └──────────────────┘                       │
│                                             │
│  All the files the agent read, the          │
│  commands it ran, its internal reasoning    │
│  — none of that pollutes the main context.  │
└─────────────────────────────────────────────┘
```

Each agent:
- Gets a **fresh context window** when it starts
- Can read files, run commands, and use tools independently
- Returns only its **final result** to the caller
- Its internal work is **discarded** after completion

---

## Skill Injection: The Best of Both Worlds

Here's the powerful part: agents can **inject skills** into their context. This means:

1. You keep skills as reusable knowledge modules
2. Agents pull in only the skills they need
3. The skill knowledge lives in the agent's isolated context

```
┌───────────────────────────────────────────┐
│           implementer agent               │
│                                           │
│  Frontmatter:                             │
│    skills:                                │
│      - implement-ticket                   │
│                                           │
│  ┌─────────────────────────────────┐      │
│  │  implement-ticket SKILL.md      │      │
│  │  is loaded into this agent's    │      │
│  │  context automatically          │      │
│  └─────────────────────────────────┘      │
│                                           │
│  The agent now has implementation         │
│  knowledge without polluting main context │
└───────────────────────────────────────────┘
```

---

## Create the Implementer Agent

Let's create an agent that injects the `implement-ticket` skill and runs in isolation.

Use the `/agent-creator` meta-skill to generate the agent. In Claude Code, type:

```
/agent-creator Create an agent called "implementer"

The agent should:
- Implement DEV subtasks for a given Jira ticket
- Inject the "implement-ticket" skill for implementation knowledge
- Accept a Jira ticket key as input (e.g., PROJECT-123)
- Find all [DEV] subtasks linked to that ticket
- Implement each subtask following the injected skill's instructions
- Transition completed subtasks to "Done" status
- Add comments to Jira summarizing what was implemented
- Have full write access (Read, Write, Edit, Bash, Glob, Grep)

Output format - return a structured summary:
- Ticket key
- List of subtasks completed with brief descriptions
- Files changed
- Test results if applicable
- Overall status (Complete / Partial with reason)
```

**Review the generated agent:**

```bash
cat .claude/agents/implementer.md
```

Verify it includes:
- `skills: implement-ticket` in the frontmatter (this is the skill injection)
- The Jira MCP tools in the tools list
- Clear output format instructions

---

## Update the Deploy-Ticket Command

Now update `/deploy-ticket` to use the agent for the implementation phase.

### How Commands Invoke Agents

Commands use the **Task tool** to spawn agents as subagents. The Task tool launches the agent in an isolated context and returns only the result.

**Edit** `.claude/commands/deploy-ticket.md`:

**Step 1:** Update the `allowed-tools` in the frontmatter to include `Task`:

```yaml
---
description: Runs the full dev lifecycle for a feature
allowed-tools: Read, Write, Bash, Task, Glob, Grep
argument-hint: <feature description in project PROJECT_KEY>
---
```

**Step 2:** Update the IMPLEMENT PHASE section:

**Before (using skill directly):**
```markdown
3. IMPLEMENT PHASE — Use the "implement-ticket" skill:
   - Pass it the ticket key from step 1
   - It will find and implement all [DEV] subtasks
   - Capture progress updates
```

**After (using Task tool to spawn agent):**
```markdown
3. IMPLEMENT PHASE — Spawn the "implementer" agent:
   - Use the Task tool to launch the "implementer" agent as a subagent
   - Pass the ticket key from step 1 as the task input
   - The agent runs in its own isolated context window
   - The agent injects the implement-ticket skill automatically
   - Wait for the agent to complete and return its implementation summary
   - Capture the summary for the final report
```

The key difference: instead of Claude following the skill instructions in the main context, the Task tool spawns a separate agent process that handles the implementation work independently.

---

## Test the Agent-Based Workflow

**Step 1: Clear your context**

```
/clear
```

**Step 2: Check baseline context**

```
/context
```

Note the starting context size.

**Step 3: Run deploy-ticket with the agent**

```
/deploy-ticket Add a wishlist feature to PROJECT_KEY
```

(Replace PROJECT_KEY with your Jira project key)

**Step 4: Check context after**

```
/context
```

Compare to before. The implementation phase ran in an isolated context — only the summary came back. Your main context stayed cleaner.

---

## The Hybrid Architecture

You now have a hybrid setup:

```
                              ┌──────────────┐
                              │   Jira MCP   │  ← Tool (capability layer)
                              └──────┬───────┘
                                     │
          ┌──────────┬───────────────┼───────────────┐
          ▼          ▼               ▼               ▼
   ┌────────────┐ ┌────────────┐ ┌─────────────┐ ┌─────────┐
   │  create-   │ │  expand-   │ │ implement-  │ │   qa-   │  ← Skills
   │  ticket    │ │  ticket    │ │ ticket      │ │  ticket │    (knowledge)
   └─────┬──────┘ └─────┬──────┘ └──────┬──────┘ └────┬────┘
         │              │               │              │
         │              │               ▼              │
         │              │        ┌─────────────┐       │
         │              │        │ implementer │       │   ← Agent
         │              │        │   agent     │       │     (isolated)
         │              │        │ (injects    │       │
         │              │        │  skill)     │       │
         │              │        └──────┬──────┘       │
         │              │               │              │
         └──────────────┼───────────────┼──────────────┘
                        ▼               ▼
               ┌─────────────────────────────┐
               │      /deploy-ticket         │  ← Command (orchestration)
               └─────────────────────────────┘
```

**Skills run directly:** create-ticket, expand-ticket, qa-ticket (in main context)
**Agent runs isolated:** implementer (own context, injects implement-ticket skill)

---

## Checkpoint

You've now:

- [ ] Observed context growth with `/context`
- [ ] Understood the difference between skills and agents
- [ ] Created the `implementer` agent with skill injection
- [ ] Updated `/deploy-ticket` to use the Task tool to spawn the agent
- [ ] Tested the hybrid workflow and compared context usage

---

## Key Takeaways

1. **Context isolation** — Agents run in their own context, keeping main context clean
2. **Skill injection** — Agents can load skills as reusable knowledge modules
3. **Task tool** — Commands use the Task tool to spawn agents as isolated subagents
4. **Hybrid approach** — Mix skills and agents based on context impact
5. **Same knowledge, different execution** — The implement-ticket skill is reused, just executed in isolation

---

## What's Next

In **Workshop 2**, you'll build a complete three-agent architecture:

- **Planner agent** — Analyzes codebase and creates implementation plans
- **TDD Guide agent** — Implements features using test-driven development
- **Code Reviewer agent** — Validates work against the plan

Each agent runs in isolation, injecting specialized skills, and coordinated by a single `/orchestrate` command.

---

Continue to **[Workshop 2](../2-cc-workshop-2/)** for: TDD workflows, custom agents, and advanced orchestration
