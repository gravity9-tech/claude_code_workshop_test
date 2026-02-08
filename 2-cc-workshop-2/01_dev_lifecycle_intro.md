# Workshop 01: Understanding the Development Lifecycle

**Duration: ~10 minutes**

## What You'll Learn

- The Plan → TDD → Review cycle and why each phase matters
- How skills, agents, and commands form three distinct layers
- What we're building in this workshop and how the pieces connect

---

## The Development Lifecycle

Every well-run development task follows a repeatable cycle:

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│   Plan   │ ──► │   TDD    │ ──► │  Review  │
│          │     │          │     │          │
│ Break    │     │ Write    │     │ Check    │
│ work     │     │ tests    │     │ against  │
│ into     │     │ first,   │     │ plan &   │
│ steps    │     │ then     │     │ TDD      │
│          │     │ code     │     │ standards│
└──────────┘     └──────────┘     └──────────┘
```

**Plan** — Before writing any code, break the task into concrete steps. Identify which files to touch, define acceptance criteria, and set clear boundaries for what "done" looks like.

**TDD** — Write failing tests first (red), implement just enough code to make them pass (green), then clean up (refactor). The tests become living documentation of the requirements.

**Review** — Check the implementation against the original plan. Verify TDD discipline was followed. Catch issues before they make it further downstream.

Each phase feeds the next: the plan guides what tests to write, the tests guide what code to write, and the review validates everything against the plan.

---

## Three Layers: Skills, Agents, Commands

In Workshop 1, you learned about **skills** — markdown instructions that teach Claude how to do things. Now we'll add two more layers:

| Layer | What It Is | Role | Example |
|-------|-----------|------|---------|
| **Skill** | Markdown knowledge file | Encodes *how* to do something | `tdd-workflow` — TDD methodology |
| **Agent** | Isolated executor with its own context | Performs a focused job using skills and tools | `planner` — creates implementation plans |
| **Command** | Slash command that orchestrates | Chains agents into a workflow | `/orchestrate` — runs the full cycle |

### How they relate

```
Skills teach methodology
    │
    ▼
Agents apply that methodology to do focused work
    │
    ▼
Commands chain agents into end-to-end workflows
```

**Skills** are reusable knowledge. An agent can inject a skill to gain that knowledge.

**Agents** run in their own context window — isolated from the main conversation. This means they can do focused work without polluting your main session with noise.

**Commands** are what you actually invoke. One command can orchestrate multiple agents in sequence, passing context between them.

---

## What We're Building

Here's each piece we'll create and its role in the lifecycle:

### Skill: `tdd-workflow`

Encodes TDD methodology — red-green-refactor patterns, test-first principles, and refactoring guidelines. Built first because agents consume it.

### Agent: `planner`

Takes a task description and produces an implementation plan:
- Breaks work into concrete steps
- Identifies files to create or modify
- Defines acceptance criteria
- Outputs a structured plan document

### Agent: `tdd-guide`

Takes the planner's output and implements using TDD. Injects the `tdd-workflow` skill:
- Writes failing tests based on the plan's acceptance criteria (red)
- Implements code to make tests pass (green)
- Refactors while keeping tests green (refactor)

### Agent: `code-reviewer`

Takes the implementation and reviews it:
- Checks code against the original plan
- Verifies TDD discipline (tests exist, tests pass)
- Flags issues and suggests improvements

### Command: `/orchestrate`

Chains everything together:

```
/orchestrate "Add a product rating feature"
                    │
                    ▼
            ┌──────────────┐
            │   planner    │  → produces implementation plan
            └──────┬───────┘
                   │ plan
                   ▼
            ┌──────────────┐
            │  tdd-guide   │  → implements with TDD (+tdd-workflow skill)
            └──────┬───────┘
                   │ implementation
                   ▼
            ┌──────────────┐
            │code-reviewer │  → reviews against plan & standards
            └──────────────┘
```

---

## How the Pieces Connect

```
Setup → Concept Introduction
              │
              ▼
      ┌───────────────┐
      │  tdd-workflow  │  ← Skill (knowledge layer)
      │    (skill)     │
      └───────┬───────┘
              │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
┌────────┐ ┌─────────┐ ┌─────────────┐
│planner │ │tdd-guide│ │code-reviewer│  ← Agents (execution layer)
│        │ │(+skill) │ │             │
└───┬────┘ └───┬─────┘ └──────┬──────┘
    │          │              │
    └──────────┼──────────────┘
               ▼
      ┌────────────────┐
      │  /orchestrate   │  ← Command (orchestration layer)
      └───────┬────────┘
              ▼
        Full Workflow
```

The skill is the foundation — it exists so agents don't need TDD knowledge baked into their own instructions. The agents are independent workers, each responsible for one phase. The command ties them together so you can run the entire lifecycle with a single invocation.

---

## Next Up

Continue to: [02_tdd_workflow_skill.md](./02_tdd_workflow_skill.md) — Build the TDD Workflow Skill
