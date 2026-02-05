# Workshop 00: Setup & Verification

**Duration: ~5 minutes**

## What You'll Do

- Verify Workshop 1 completion
- Confirm project state and pre-built skills
- Preview what we're building in this workshop

---

## Prerequisites

Before starting, ensure you have completed **[Workshop 1](../cc-workshop-1/)** in full.

You should already have:

- **Claude Code** installed and working
- **Tea Store project** downloaded and running
- **CLAUDE.md** generated via `/init`
- **Jira MCP** configured and connected
- **Workshop 1 skills** created (create-ticket, expand-ticket, implement-ticket, qa-ticket)

---

## Step 1: Verify Claude Code

Open a terminal in your Tea Store project folder and run:

```bash
claude --version
```

You should see version output like `claude 2.x.x`.

---

## Step 2: Verify Project State

Confirm the following files exist in your project:

```bash
ls CLAUDE.md
ls .claude/skills/
```

You should see:

```
.claude/skills/
├── skill-creator/       # Pre-built: creates domain skills
├── command-creator/     # Pre-built: creates slash commands
├── agent-creator/       # Pre-built: creates custom agents
├── create-ticket/       # Workshop 1: BDD ticket creation
├── expand-ticket/       # Workshop 1: DEV + QA subtasks
├── implement-ticket/    # Workshop 1: implement & transition
└── qa-ticket/           # Workshop 1: Playwright testing
```

> **Important:** The `agent-creator` skill is essential for this workshop — we'll use it to create all three agents.

---

## Step 3: Verify MCP Servers

Start Claude Code and check MCP status:

```bash
claude
```

Then inside Claude Code, type:

```
/mcp
```

Confirm that your Jira MCP server is connected and healthy.

---

## What We're Building

In this workshop, you'll build a complete development lifecycle automation with three layers:

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

**Skill (knowledge layer):** `tdd-workflow` encodes TDD methodology so agents can follow red-green-refactor patterns.

**Agents (execution layer):** Three specialized agents each handle one phase of the development cycle:
- `planner` — Breaks work into steps with acceptance criteria
- `tdd-guide` — Implements using test-driven development (injects the skill)
- `code-reviewer` — Reviews code against the plan and TDD standards

**Command (orchestration layer):** `/orchestrate` chains all three agents into a single workflow.

---

## Checkpoint

Before continuing, verify:

- [ ] Claude Code is installed and working
- [ ] Tea Store project is set up
- [ ] `CLAUDE.md` exists in project root
- [ ] `.claude/skills/` contains pre-built creators (skill-creator, command-creator, agent-creator)
- [ ] Workshop 1 skills are present (create-ticket, expand-ticket, implement-ticket, qa-ticket)
- [ ] Jira MCP server is connected

---

## Next Up

Continue to: [01_dev_lifecycle_intro.md](./01_dev_lifecycle_intro.md) — Understanding the Development Lifecycle
