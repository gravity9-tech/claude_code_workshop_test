# Workshop 00: Setup & Verification

**Duration: ~5 minutes**

## What You'll Do

- Verify Workshop 1 completion
- Confirm project state and pre-built skills
- Preview what we're building in this workshop

---

## Prerequisites

1. Download the project zip file: https://github.com/gravity9-tech/claude_code_workshop/archive/refs/heads/main.zip

2. Extract the zip file to a location of your choice

3. Open the extracted folder in your favorite text editor

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
CLAUDE.md
.claude/skills/
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

### If No MCP Server is Found

If you don't see a Jira MCP server connected, run the setup command:

```
/create-jira-mcp create jira mcp server for this project
```

Follow the prompts to configure your Jira connection, then verify with `/mcp` again.

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

## Next Up

Continue to: [01_dev_lifecycle_intro.md](./01_dev_lifecycle_intro.md) — Understanding the Development Lifecycle
