# Workshop 07: Creating the Orchestrate Command

**Duration: ~10 minutes**

## What You'll Learn

- What slash commands are and how they differ from agents
- How commands orchestrate multiple agents into a workflow
- How to use the `/command-creator` to build `/orchestrate`

---

## Why a Command?

You now have three agents that each handle one phase of the development lifecycle. But running them manually one at a time — copying the planner's output into the tdd-guide, then copying that output into the code-reviewer — is tedious.

**Slash commands** solve this. A command is a markdown file that Claude executes when you type `/command-name`. It can chain agents together, passing context between them automatically.

| | Skills | Agents | Commands |
|---|--------|--------|----------|
| **What they are** | Knowledge files | Isolated executors | Orchestration scripts |
| **How invoked** | Auto-triggered or injected | Called by commands or directly | User types `/command-name` |
| **Format** | `SKILL.md` | `AGENT.md` | `command-name.md` |
| **Location** | `.claude/skills/` | `.claude/agents/` | `.claude/commands/` |
| **Purpose** | Teach *how* | *Do* focused work | *Chain* work together |

---

## Command Structure

Every command is a markdown file in `.claude/commands/`:

```
.claude/commands/
└── orchestrate.md
```

The file has two parts:

1. **YAML frontmatter** — metadata (description, tools, argument hint)
2. **Markdown body** — workflow instructions Claude follows

```yaml
---
description: Does something useful
allowed-tools: Read, Write, Task
argument-hint: <description of expected arguments>
---

# Command Title

## Arguments
- `$ARGUMENTS` - What the user passes after the command name

## Workflow
1. First, do this...
2. Then, do that...
```

When a user types `/orchestrate Add a rating feature`, the text after the command name becomes `$ARGUMENTS`.

---

## What `/orchestrate` Does

The command chains all three agents in sequence, passing context between them:

```
User: /orchestrate "Add a product rating feature"
                    │
                    │  $ARGUMENTS = task description
                    ▼
           ┌──────────────┐
           │   planner    │  Agent 1: analyze & plan
           │  (read-only) │
           └──────┬───────┘
                  │
                  │  output: implementation plan
                  ▼
           ┌──────────────┐
           │  tdd-guide   │  Agent 2: implement with TDD
           │ (+tdd-workflow│
           │   skill)     │
           └──────┬───────┘
                  │
                  │  output: implementation summary
                  ▼
           ┌──────────────┐
           │code-reviewer │  Agent 3: review & validate
           │  (read-only) │
           └──────┬───────┘
                  │
                  │  output: review verdict
                  ▼
           Final report to user
```

Each agent runs in its own context window. The command passes the relevant output from one agent as input to the next.

---

## Create the Command

Use the `/command-creator` to generate the orchestrate command. In your Claude Code session, type:

```
/command-creator Create a slash command called "orchestrate"

The command should chain three agents in sequence to run a complete
development lifecycle:

1. PLAN PHASE — Run the "planner" agent:
   - Pass it the task description from $ARGUMENTS
   - Capture the implementation plan it produces

2. IMPLEMENT PHASE — Run the "tdd-guide" agent:
   - Pass it the implementation plan from step 1
   - Capture the implementation summary (files changed, tests written,
     test results)

3. REVIEW PHASE — Run the "code-reviewer" agent:
   - Pass it both the original plan (from step 1) and the
     implementation summary (from step 2)
   - Capture the review verdict

4. REPORT — Output a final summary containing:
   - The original plan (condensed)
   - What was implemented
   - The review verdict (PASS / PASS WITH NOTES / FAIL)
   - Any issues found

The command should:
- Use the Task tool to spawn each agent as a subagent
- Run agents sequentially (each depends on the previous output)
- If the planner fails to produce a plan, stop and report the error
- If the tdd-guide encounters test failures it cannot resolve, continue
  to the review phase so the reviewer can document the issues
- The argument hint should be: <task or feature description>
```

---

## Review the Generated Command

Once the `/command-creator` finishes, review what was generated:

```bash
# macOS/Linux
cat .claude/commands/orchestrate.md

# Windows
type .claude\commands\orchestrate.md
```

Verify the generated command includes:

**Frontmatter:**
- `description:` explains what the command does
- `allowed-tools:` includes `Task` (required for spawning agents)
- `argument-hint:` shows the expected input format

**Body:**
- `$ARGUMENTS` is used as the task description
- Three phases in order: planner → tdd-guide → code-reviewer
- Context is passed between agents (plan → implementation → review)
- Final output format with summary and verdict

The structure should look something like:

```yaml
---
description: Run a full dev lifecycle — plan, implement with TDD, and review
allowed-tools: Task, Read, Glob, Grep
argument-hint: <task or feature description>
---

# Orchestrate: Full Development Lifecycle

Run a complete plan → implement → review cycle for a given task.

## Arguments

- `$ARGUMENTS` - The task or feature to implement

## Workflow

### Phase 1: Plan
1. Spawn the **planner** agent with the task description
2. Capture the implementation plan

### Phase 2: Implement
3. Spawn the **tdd-guide** agent with the plan from Phase 1
4. Capture the implementation summary

### Phase 3: Review
5. Spawn the **code-reviewer** agent with the plan and implementation
6. Capture the review verdict

### Phase 4: Report
7. Output a final summary with plan, implementation, and verdict
```

---

## Test the Command

Try running the command with a small feature. In Claude Code:

```
/orchestrate Add a "last updated" timestamp to the product detail page
that shows when the product data was last modified
```

Watch the three phases execute in sequence:
1. The planner analyzes the codebase and produces a plan
2. The tdd-guide writes tests and implements the feature
3. The code reviewer validates the work and issues a verdict

The final output should include a summary of all three phases.

---

## Architecture So Far

```
Setup → Concept Introduction
              │
              ▼
      ┌───────────────┐
      │  tdd-workflow  │  ← Skill (knowledge layer) ✅
      │    (skill)     │
      └───────┬───────┘
              │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
┌────────┐ ┌─────────┐ ┌─────────────┐
│planner │ │tdd-guide│ │code-reviewer│  ← Agents (execution layer) ✅
│  ✅    │ │  ✅     │ │     ✅      │
└───┬────┘ └───┬─────┘ └──────┬──────┘
    │          │              │
    └──────────┼──────────────┘
               ▼
      ┌────────────────┐
      │  /orchestrate   │  ← Command (orchestration layer) ✅
      └───────┬────────┘
              ▼
        Full Workflow
```

Every layer is built. All that's left is putting it all together.

---

## Checkpoint

- [ ] Understand what slash commands are (orchestration scripts invoked with `/command-name`)
- [ ] Understand how commands differ from skills and agents
- [ ] Used `/command-creator` to generate `/orchestrate`
- [ ] Verified the command chains planner → tdd-guide → code-reviewer in sequence
- [ ] Verified context is passed between agents (plan → implementation → review)
- [ ] Tested the command with a sample feature

---

## Next Up

Continue to: [08_full_workflow.md](./08_full_workflow.md) — Full Workflow Demo
