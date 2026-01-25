# Claude Code Workshop 1: Foundations

**Total Duration: ~80 min**

A hands-on guide to getting started with Claude Code and building a complete dev lifecycle automation using Jira.

## Prerequisites

- Claude Code installed (`npm install -g @anthropic-ai/claude-code`)
- Node.js (for MCP servers)
- Jira Cloud account with API access
- Basic command line familiarity

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 00 | [Project Setup](./00_setup.md) | 5 min |
| 01 | [Context Window](./01_context_window.md) | 5 min |
| 02 | [Getting Started](./02_getting_started.md) | 8 min |
| 03 | [Jira MCP](./03_jira_mcp.md) | 15 min |
| 04 | [Skills Introduction](./04_skills_intro.md) | 5 min |
| 05 | [Skill Builder](./05_skill_builder.md) | 8 min |
| 06 | [Dev Lifecycle Skills](./06_dev_lifecycle.md) | 25 min |
| 07 | [Slash Commands](./07_slash_commands.md) | 10 min |

## Learning Path

```
00 Project Setup (clone tea-store-demo)
         |
         v
01 Context Window (memory management)
         |
         v
02 Getting Started (/init, CLAUDE.md)
         |
         v
03 Jira MCP (ticket management integration)
         |
         v
04 Skills Introduction (what skills are)
         |
         v
05 Skill Builder (explore the skill-creator)
         |
         v
06 Dev Lifecycle Skills (build 4 skills for full workflow)
   ├── create-ticket (BDD descriptions)
   ├── expand-ticket (DEV + QA subtasks)
   ├── implement-ticket (code & mark done)
   └── qa-ticket (Playwright tests & mark done)
         |
         v
07 Deploy Ticket Orchestrator (use command-creator)
   └── /deploy-ticket (orchestrates all skills)
```

---

## What You'll Learn

| Module | Feature | What It Does |
|--------|---------|--------------|
| 01 | Context Window | Understanding Claude's memory limits |
| 02 | `/init` | Generates CLAUDE.md project memory |
| 03 | Jira MCP | External tool for ticket management |
| 04 | Skills | Domain expertise for Claude |
| 05 | Skill Builder | Explore the skill-creator skill |
| 06 | Dev Lifecycle | Build a complete 4-skill workflow automation |
| 07 | Orchestrator | Use command-creator to build /deploy-ticket orchestrator |

---

## Final Project Structure

After completing Workshop 1:

```
tea-store-demo/
├── .claude/
│   ├── commands/
│   │   └── deploy-ticket.md    # Slash command: full lifecycle orchestrator
│   └── skills/
│       ├── skill-creator/      # Pre-built: creates domain skills
│       ├── command-creator/    # Pre-built: creates slash commands
│       ├── agent-creator/      # Pre-built: creates custom agents (for W2)
│       ├── create-ticket/      # Your skill: BDD ticket creation
│       ├── expand-ticket/      # Your skill: DEV + QA subtasks
│       ├── implement-ticket/   # Your skill: implement & transition
│       └── qa-ticket/          # Your skill: Playwright testing
├── .mcp.json                    # Jira MCP server config
└── CLAUDE.md                    # Project memory (via /init)
```

---

## What You'll Accomplish

By the end of this workshop, you will have:

1. **Set up Claude Code** with a real project (tea-store-demo)
2. **Connected to Jira** via MCP for ticket management
3. **Built a complete dev lifecycle automation:**
   - `create-ticket` — Skill that creates tickets with BDD acceptance criteria
   - `expand-ticket` — Skill that breaks tickets into DEV and QA subtasks
   - `implement-ticket` — Skill that implements code and transitions DEV tasks to Done
   - `qa-ticket` — Skill that runs Playwright tests and transitions QA tasks to Done
   - `/deploy-ticket` — Slash command that orchestrates all skills into a single workflow

You'll have a fully automated development workflow from idea to tested code with one command!

---

## Troubleshooting

**Claude Code not found:**
```bash
npm install -g @anthropic-ai/claude-code
```

**Jira MCP issues:**
```bash
claude mcp list
claude mcp get jira
```

**Skills not triggering:**
- Ensure your task description matches the skill's trigger words
- Check that MCP servers are running with `/mcp`

## Help

- Type `/help` in Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Docs](https://docs.anthropic.com/claude-code)

---

Start with [00_setup.md](./00_setup.md)

Continue to **Workshop 2** for: Advanced Playwright, Slash Commands, Custom Agents, Hooks, and Full Workflow orchestration
