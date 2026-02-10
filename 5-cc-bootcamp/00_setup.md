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

> **Important:** The `agent-creator`, `skill-creator`, `command-creator` skills are essential for this workshop — we'll use them to create required skills, subagents and slash commands.

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

This bootcamp takes you from understanding the building blocks of Claude Code all the way to packaging, distributing, and running them autonomously. Here's the journey:

```
┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐
│ Module 1  │──►│ Module 2  │──►│ Module 3  │──►│ Module 4  │──►│ Module 5  │──►│Module 6-7 │
│           │   │           │   │           │   │           │   │           │   │           │
│ Context   │   │ Build     │   │ Understand│   │ Package & │   │ Automate  │   │ Plan your │
│ Eng. Why  │   │ Components│   │ Plugins & │   │ Distribute│   │ with Ralph│   │ team's    │
│           │   │           │   │ Marketplace│  │           │   │ & LISA    │   │ journey   │
└───────────┘   └───────────┘   └───────────┘   └───────────┘   └───────────┘   └───────────┘
```

### Module 2: Key Components — Build the Feature Lifecycle

You'll build a complete feature implementation pipeline using all four component types:

```
                              ┌──────────────────┐
                              │  Jira MCP Server │  ← MCP Tool (capability layer)
                              └────────▲─────────┘
                                  ▲    │    ▲
                           ┌──────┘    │    └──────┐
                           │           │           │
    ┌──────────┐ ┌─────────┴┐ ┌───────┴──┐ ┌──────┴─────┐ ┌──────────────┐
    │ Create   │ │ Expand   │ │  TDD     │ │  Create    │ │ Commit &     │
    │ Ticket   │ │ Ticket   │ │ Workflow │ │  Branch    │ │ Merge        │ ← Skills
    │ Skill    │ │ Skill    │ │ Skill    │ │  Skill     │ │ Skill        │   (knowledge layer)
    └─────▲────┘ └────▲─────┘ └────▲─────┘ └─────▲──────┘ └──────▲──────┘
          │           │            │              │               │
          └─────┬─────┘            │              │               │
                │                  │              │               │
         ┌──────┴──────┐   ┌──────┴──────────────┴──┐   ┌───────┴────────┐
         │   Planner   │   │    TDD Implementer     │   │    Code        │
         │   Agent     │   │    Agent               │   │  Reviewer      │ ← Agents
         │             │   │                        │   │   Agent        │   (execution layer)
         └──────▲──────┘   └───────────▲────────────┘   └───────▲────────┘
                │                      │                        │
                └──────────┬───────────┴────────────────────────┘
                           │
                ┌──────────┴───────────┐
                │  /implement-feature  │  ← Slash Command (orchestration layer)
                │   Slash Command      │
                └──────────────────────┘
```

**Skills** — `create-ticket`, `expand-ticket`, `tdd-workflow`, `create-branch`, `commit-and-merge`
**Agents** — `planner`, `tdd-implementer`, `code-reviewer`
**Slash command** — `/implement-feature` chains all three agents into one workflow

### Module 3: Marketplace Intro — Why Distribution Matters

Understand how plugins package components into shareable units and how the three-tier marketplace (organisation, business line, team) organises them at scale.

### Module 4: Plugins & Marketplace Architecture — Package and Distribute

Build two new skills (`create-plugin`, `create-marketplace`) to package Module 2's components into an installable plugin, register it in a marketplace, and learn how to install plugins using `/plugin add`.

### Module 5: Ralph Loop & LISA — Autonomous Execution

Run features autonomously using the Ralph Loop (a self-correcting iteration loop with a Stop Hook) and LISA (an interview-driven spec generator). Lisa plans, Ralph does — together they implement a complete feature without manual intervention.

### Modules 6–7: Context Engineering How & Champion as Publisher

Map your own team's context gaps, build an action plan, and define your ongoing role as an AI Champion maintaining your team's marketplace.

---

## Next Up

Continue to: [01_context_engineering_why.md](./01_context_engineering_why.md) — Understanding the why we require context engineering process
