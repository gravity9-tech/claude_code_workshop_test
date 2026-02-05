# Claude Code Workshops

Hands-on workshops for learning Claude Code — from skills and MCP integrations to custom agents and orchestration.

## Prerequisites

- **Python 3.9+** for the backend
- **Node.js 18+** for the frontend and Claude Code
- Claude Code installed:
  ```bash
  npm install -g @anthropic-ai/claude-code
  ```
- Jira Cloud account with API access
- Basic command line familiarity

## Workshops

| Workshop | Level | Duration | Description |
|----------|-------|----------|-------------|
| [Workshop 1](./cc-workshop-1/) | Foundations | ~70 min | Setup, Jira MCP, skills, and `/deploy-ticket` command |
| [Workshop 2](./cc-workshop-2/) | Advanced | ~85 min | TDD skill, custom agents, and `/orchestrate` command |

## Learning Path

```
Workshop 1: Foundations
│
│  Setup → Context Window → Getting Started → Jira MCP
│                                                │
│                                                ▼
│               ┌──────────────┐
│               │   Jira MCP   │  ← Tool
│               └──────┬───────┘
│                      │
│       ┌──────┬───────┼───────┬──────┐
│       ▼      ▼       ▼       ▼      │
│    create  expand  implement  qa    │  ← Skills
│    ticket  ticket  ticket   ticket  │
│       │      │       │       │      │
│       └──────┼───────┼───────┘      │
│              ▼       ▼              │
│         /deploy-ticket              │  ← Command
│                                     │
│  [You now have a Jira-driven        │
│   dev lifecycle]                    │
│                                     │
└─────────────────────────────────────┘
              │
              ▼
Workshop 2: Advanced
│
│  Setup → Dev Lifecycle Intro
│              │
│              ▼
│      ┌───────────────┐
│      │  tdd-workflow  │  ← Skill
│      └───────┬───────┘
│              │
│    ┌─────────┼─────────┐
│    ▼         ▼         ▼
│ planner  tdd-guide  code-reviewer   ← Agents
│    │         │         │
│    └─────────┼─────────┘
│              ▼
│       /orchestrate                  ← Command
│
└─────────────────────────────────────┘
```

## Target Project

Both workshops use [tea-store-demo](https://github.com/gravity9-tech/claude_code_workshop), a full-stack e-commerce application:

- **Backend**: FastAPI, Python, Pydantic
- **Frontend**: React 19, TypeScript, Tailwind CSS
- **Testing**: Pytest, Vitest, Playwright

## Quick Start

1. Complete Workshop 1 first if you're new to Claude Code
2. Start with [cc-workshop-1/00_setup.md](./cc-workshop-1/00_setup.md)

## Help

- Type `/help` inside Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
