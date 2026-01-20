# Claude Code Workshop 2: Advanced Features

**Total Duration: ~60 min**

Build and test features using ticket-driven development with Playwright automation.

## Prerequisites

- **Completed Workshop 1** (or equivalent knowledge)
- A **planned Jira ticket** from Workshop 1 (with P1/P2/QA subtasks)
- Claude Code installed with dev-tools plugin
- Jira MCP configured and working

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 01 | [Playwright MCP](./01_playwright_mcp.md) | 10 min |
| 02 | [Slash Commands](./02_slash_commands.md) | 8 min |
| 03 | [Custom Agents](./03_custom_agents.md) | 10 min |
| 04 | [Implement Ticket](./04_implement_ticket.md) | 10 min |
| 05 | [QA Ticket](./05_qa_ticket.md) | 10 min |
| 06 | [Hooks](./06_hooks.md) | 6 min |
| 07 | [Full Workflow](./07_full_workflow.md) | 8 min |

## Learning Path

```
01 Playwright MCP (browser automation)
         |
         v
02 Slash Commands (reusable prompts)
         |
         v
03 Custom Agents (specialized workers)
         |
         v
04 Implement Ticket (phase-by-phase building)
         |
         v
05 QA Ticket (automated BDD testing)
         |
         v
06 Hooks (event automation)
         |
         v
07 Full Workflow (/build-and-qa orchestration)
```

---

## What You'll Learn

| Module | Feature | What It Does |
|--------|---------|--------------|
| 01 | Playwright MCP | Browser automation for testing |
| 02 | Slash Commands | Reusable prompts via `/name` |
| 03 | Custom Agents | Specialized subagents with skills |
| 04 | Implement Ticket | Build features phase-by-phase |
| 05 | QA Ticket | Automated testing against BDD scenarios |
| 06 | Hooks | Shell commands triggered by events |
| 07 | Full Workflow | End-to-end `/build-and-qa` |

---

## What You'll Accomplish

By the end of this workshop, you will have:

1. **Set up Playwright MCP** for browser automation
2. **Created a slash command** for running tests
3. **Built a custom agent** that uses skills
4. **Implemented your ticket** using the implement-ticket skill
5. **Run automated QA tests** against BDD acceptance criteria
6. **Added hooks** for file protection
7. **Used the full workflow** `/build-and-qa` command

---

## Final Project Structure

After completing Workshop 2:

```
pandora-demo/
├── .claude/
│   ├── commands/
│   │   └── test-feature.md        # Custom test command
│   ├── agents/
│   │   └── feature-builder.md     # Custom agent
│   └── settings.json              # Hooks config
├── .mcp.json                      # MCP servers (Jira + Playwright)
├── CLAUDE.md                      # Project memory
└── [your new feature code]        # Implemented from ticket!
```

---

## Troubleshooting

**Playwright not working:**
```bash
claude mcp list
claude mcp get playwright
# May need: npx @playwright/mcp --help
```

**Implement-ticket fails:**
- Check ticket status is "Selected for Development"
- Ensure P1/P2 subtasks exist

**QA tests fail:**
- Is the app running? (`python main.py`)
- Check browser is launching (Playwright)
- Review the BDD scenario in QA subtask

**Hooks not triggering:**
- Restart Claude Code after changing hooks
- Check `.claude/settings.json` syntax

## Help

- Type `/help` in Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Docs](https://docs.anthropic.com/claude-code)

---

Start with [01_playwright_mcp.md](./01_playwright_mcp.md)
