# Claude Code Bootcamp: AI Champions — Context Engineering & Marketplace

**Total Duration: ~4 hours**

A hands-on bootcamp that takes AI Champions from understanding context engineering through building reusable components, packaging them into plugins, distributing them via a marketplace, and running them autonomously — culminating in a personal action plan for your team.

## Prerequisites

- Completed [Workshop 1](../1-cc-workshop-1/) and [Workshop 2](../2-cc-workshop-2/)
- Claude Code installed (`claude --version` should show `2.x.x`)
- Tea Store project (`claude_code_workshop`) cloned and runnable
- Jira MCP server configured
- Pre-built skills available: `skill-creator`, `agent-creator`, `command-creator`

## Bootcamp Modules

| Module | Topic | Duration | Format |
|--------|-------|----------|--------|
| 00 | [Setup & Verification](./00_setup.md) | 5 min | Hands-on |
| 01 | [Context Engineering — The Why](./01_context_engineering_why.md) | 20 min | Presentation + Discussion |
| 02 | [Key Components](./02_key_components.md) | 60 min | Hands-on Workshop |
| 03 | [Marketplace Intro](./03_marketplace_intro.md) | 20 min | Presentation + Discussion |
| 04 | [Plugins & Marketplace Architecture](./04_plugins_marketplace_architecture.md) | 45 min | Hands-on Workshop |
| 05 | [Ralph Loop & LISA](./05_ralph_loop_and_lisa.md) | 30 min | Presentation + Demo |
| 06 | [Context Engineering — The How](./06_context_engineering_how.md) | 45 min | Walkthrough + Exercise |
| 07 | [Champion as Publisher](./07_champion_as_publisher.md) | 20 min | Discussion + Planning |

## Agenda

```
┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐
│ Module 1  │──►│ Module 2  │──►│ Module 3  │──►│ Module 4  │──►│ Module 5  │──►│Module 6-7 │
│           │   │           │   │           │   │           │   │           │   │           │
│ Context   │   │ Build     │   │ Understand│   │ Package & │   │ Automate  │   │ Plan your │
│ Eng. Why  │   │ Components│   │ Plugins & │   │ Distribute│   │ with Ralph│   │ team's    │
│           │   │           │   │ Marketplace│  │           │   │ & LISA    │   │ journey   │
└───────────┘   └───────────┘   └───────────┘   └───────────┘   └───────────┘   └───────────┘
```

### Module 1: Context Engineering — The Why

Understand why context engineering matters more than prompt engineering. Learn the four-phase process and the three-tier marketplace (Organisation, Business Line, Team).

### Module 2: Key Components — Build the Feature Lifecycle

Build a complete feature implementation pipeline using all four component types:

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
    └─────▲────┘ └────▲─────┘ └────▲─────┘ └─────▲──────┘ └──────▲───────┘
          │           │            │             │               │
          └─────┬─────┘            │             │               │
                │                  │             │               │
         ┌──────┴──────┐   ┌──────┴─────────────┴──┐   ┌───────┴────────┐
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

**Skills:** `create-ticket`, `expand-ticket`, `tdd-workflow`, `create-branch`, `commit-and-merge`
**Agents:** `planner`, `tdd-implementer`, `code-reviewer`
**Slash command:** `/implement-feature` chains all three agents into one workflow

### Module 3: Marketplace Intro

Understand how plugins package components into shareable units and how the three-tier marketplace (Organisation, Business Line, Team) organises them at scale.

### Module 4: Plugins & Marketplace Architecture

Build two new skills (`create-plugin`, `create-marketplace`), package Module 2's components into a `feature-lifecycle` plugin and creator skills into a `meta-skills` plugin, register them in a marketplace, and learn how to install plugins using `/plugin install`.

### Module 5: Ralph Loop & LISA

Run features autonomously using the Ralph Loop (a self-correcting iteration loop with a Stop Hook) and LISA (an interview-driven spec generator). LISA plans, Ralph does — together they implement a complete feature without manual intervention.

### Modules 6–7: Context Engineering How & Champion as Publisher

Map your own team's context gaps, build an action plan, and define your ongoing role as an AI Champion maintaining your team's marketplace.

---

## What You'll Build

| # | Component | Type | Module |
|---|-----------|------|--------|
| 1 | `create-ticket` | Skill | 2 |
| 2 | `expand-ticket` | Skill | 2 |
| 3 | `tdd-workflow` | Skill | 2 |
| 4 | `create-branch` | Skill | 2 |
| 5 | `commit-and-merge` | Skill | 2 |
| 6 | `planner` | Agent | 2 |
| 7 | `tdd-implementer` | Agent | 2 |
| 8 | `code-reviewer` | Agent | 2 |
| 9 | `/implement-feature` | Command | 2 |
| 10 | `create-plugin` | Skill | 4 |
| 11 | `create-marketplace` | Skill | 4 |
| 12 | `feature-lifecycle` | Plugin | 4 |
| 13 | `meta-skills` | Plugin | 4 |

---

## Final Project Structure

After completing the bootcamp:

```
tea-store-demo/
├── .claude/
│   ├── agents/
│   │   ├── planner.md              # Agent: creates tickets, plans, outputs summary table
│   │   ├── tdd-implementer.md      # Agent: fetches subtasks from Jira, implements on one branch
│   │   └── code-reviewer.md        # Agent: reviews, commits & merges if no critical issues
│   ├── commands/
│   │   └── implement-feature.md    # Slash command: full lifecycle orchestrator
│   └── skills/
│       ├── skill-creator/          # Pre-built: creates domain skills
│       ├── agent-creator/          # Pre-built: creates custom agents
│       ├── command-creator/        # Pre-built: creates slash commands
│       ├── create-ticket/          # Module 2: creates Jira tickets with BDD
│       ├── expand-ticket/          # Module 2: breaks tickets into subtasks
│       ├── tdd-workflow/           # Module 2: TDD methodology knowledge
│       ├── create-branch/          # Module 2: creates feature branches
│       ├── commit-and-merge/       # Module 2: commits and merges to main
│       ├── create-plugin/          # Module 4: packages components into plugins
│       └── create-marketplace/     # Module 4: builds marketplace catalogue
├── .claude-plugin/
│   └── marketplace.json            # Module 4: marketplace catalogue
├── plugins/
│   ├── feature-lifecycle/          # Module 4: feature implementation pipeline plugin
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── skills/
│   │   ├── agents/
│   │   ├── commands/
│   │   └── mcp/
│   └── meta-skills/                # Module 4: creator skills plugin
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
├── .mcp.json                        # Jira MCP server config
└── CLAUDE.md                        # Project memory
```

---

## Troubleshooting

**Claude Code not found:**
```bash
npm install -g @anthropic-ai/claude-code
```

**Skills not triggering:**
- Ensure your task description matches the skill's trigger words
- Check that MCP servers are running with `/mcp`

**Agents not found:**
- Verify `.claude/agents/` directory exists
- Check agent file names match expected format

**Plugins not installing:**
- Verify `.claude-plugin/marketplace.json` exists and is valid JSON
- Run `/plugin validate .` to check for issues
- Ensure plugin folders contain `.claude-plugin/plugin.json`

## Help

- Type `/help` in Claude Code
- Type `/mcp` to check MCP server status
- Type `/skills` to list available skills
- [Claude Code Docs](https://code.claude.com/docs)

---

Start with [00_setup.md](./00_setup.md)
