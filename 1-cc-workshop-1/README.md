# Claude Code Workshop 1: Foundations

**Total Duration: ~85 min**

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
| 05 | [Dev Lifecycle Skills](./05_qa_lifecycle.md) | 25 min |
| 06 | [Deploy Ticket Command](./06_slash_commands.md) | 10 min |
| 07 | [Introduction to Agents](./07_agents_intro.md) | 15 min |

## Learning Path

```
Project Setup → Context Window → Getting Started
                                       │
                                       ▼
                              ┌──────────────┐
                              │   Jira MCP   │  ← Tool (capability layer)
                              └──────┬───────┘
                                     │
          ┌──────────┬───────────────┼───────────────┐
          ▼          ▼               ▼               ▼
   ┌────────────┐ ┌────────────┐ ┌─────────────┐ ┌─────────┐
   │  create-   │ │  expand-   │ │ implement-  │ │   qa-   │  ← Skills
   │  ticket    │ │  ticket    │ │ ticket      │ │  ticket │    (knowledge
   │            │ │            │ │             │ │         │     layer)
   └─────┬──────┘ └─────┬──────┘ └──────┬──────┘ └────┬────┘
         │              │               │              │
         │              │               ▼              │
         │              │        ┌─────────────┐       │
         │              │        │ implementer │       │   ← Agent
         │              │        │   agent     │       │     (isolated
         │              │        │ (injects    │       │      context)
         │              │        │  skill)     │       │
         │              │        └──────┬──────┘       │
         │              │               │              │
         └──────────────┼───────────────┼──────────────┘
                        ▼               ▼
               ┌─────────────────────────────┐
               │      /deploy-ticket         │  ← Command (orchestration)
               └─────────────────────────────┘
```

---

## Final Project Structure

After completing Workshop 1:

```
tea-store-demo/
├── .claude/
│   ├── agents/
│   │   └── implementer.md      # Your agent: isolated implementation (injects skill)
│   ├── commands/
│   │   └── deploy-ticket.md    # Slash command: full lifecycle orchestrator
│   └── skills/
│       ├── skill-creator/      # Pre-built: creates domain skills
│       ├── command-creator/    # Pre-built: creates slash commands
│       ├── agent-creator/      # Pre-built: creates custom agents
│       ├── create-ticket/      # Your skill: BDD ticket creation
│       ├── expand-ticket/      # Your skill: DEV + QA subtasks
│       ├── implement-ticket/   # Your skill: implement & transition (also injected into agent)
│       └── qa-ticket/          # Your skill: Playwright testing
├── .mcp.json                    # Jira MCP server config
└── CLAUDE.md                    # Project memory (via /init)
```

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

Continue to **[Workshop 2](../cc-workshop-2/)** for: TDD workflows, custom agents, and advanced orchestration
