# Claude Code Workshop 1: Foundations

**Total Duration: ~45 min**

A hands-on guide to getting started with Claude Code for software development.

## Prerequisites

- Claude Code installed (`npm install -g @anthropic-ai/claude-code`)
- Node.js (for MCP servers)
- Basic command line familiarity
- This repository cloned locally

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 00 | [Project Setup](./00_setup.md) | 5 min |
| 01 | [Context Window](./01_context_window.md) | 5 min |
| 02 | [Getting Started](./02_getting_started.md) | 8 min |
| 03 | [MCP & Playwright](./03_mcp_playwright.md) | 12 min |
| 04 | [Slash Commands](./04_slash_commands.md) | 10 min |
| 05 | [Subagents](./05_subagents.md) | 10 min |

## Learning Path

```
00 Project Setup (clone & open)
         |
         v
01 Context Window (memory management)
         |
         v
02 Getting Started (/init, CLAUDE.md)
         |
         v
03 MCP & Playwright (external tools)
         |
         v
04 Slash Commands (automation)
         |
         v
05 Subagents (parallel agents)
```

---

## What You'll Learn

| Module | Feature | What It Does |
|--------|---------|--------------|
| 01 | Context Window | Understanding Claude's memory limits |
| 02 | `/init` | Generates CLAUDE.md project memory |
| 03 | MCP | External tools (browser automation) |
| 04 | Commands | Reusable prompts via `/name` |
| 05 | Agents | Autonomous parallel workers |

---

## Final Project Structure

After completing Workshop 1:

```
your-project/
├── .claude/
│   ├── commands/
│   │   └── test-basket.md
│   └── agents/
│       └── (ready for custom agents)
├── .mcp.json                    # MCP servers config
└── CLAUDE.md                    # Project memory (via /init)
```

---

## Troubleshooting

**Claude Code not found:**
```bash
npm install -g @anthropic-ai/claude-code
```

**Commands not appearing:**
```bash
ls .claude/commands/
```

**MCP server issues:**
```bash
claude mcp list
claude mcp get playwright
```

## Help

- Type `/help` in Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Docs](https://docs.anthropic.com/claude-code)

---

Start with [00_setup.md](./00_setup.md)

Continue to **Workshop 2** for: Skills, Custom Agents with Skills, and Hooks
