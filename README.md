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
| [Assignment 1](./cc-assignment-1/) | Practice | ~60-90 min | Extend the lifecycle with security review, `/full-lifecycle`, and more |

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
              │
              ▼
Assignment 1: Practice
│
│  Extend what you built:
│  ├── security-reviewer agent
│  ├── /full-lifecycle command (Jira + agents)
│  ├── documentation-agent (bonus)
│  └── auto git commits (bonus)
│
└─────────────────────────────────────┘
```

## Pre-built Meta-Skills

The target project comes with five meta-skills that workshops rely on:

| Meta-Skill | What It Does | Used In |
|------------|-------------|---------|
| `skill-creator` | Generates `SKILL.md` files from natural language descriptions | Workshop 1 & 2 |
| `command-creator` | Generates slash command `.md` files with orchestration logic | Workshop 1 & 2 |
| `agent-creator` | Generates `AGENT.md` files with tools, model, and skill injection | Workshop 2 |
| `create-jira-mcp` | Guides setup of the Jira MCP server (credentials, install, register) | Workshop 1 |
| `create-playwright-mcp` | Guides setup of the Playwright MCP server (browsers, install, register) | Workshop 1 |

These live in `.claude/skills/` and are available from the start. The creator skills generate well-structured artifacts from plain English descriptions. The MCP skills walk through tool setup step by step.

## Target Project

Both workshops use [tea-store-demo](https://github.com/gravity9-tech/claude_code_workshop), a full-stack e-commerce application:

- **Backend**: FastAPI, Python, Pydantic
- **Frontend**: React 19, TypeScript, Tailwind CSS
- **Testing**: Pytest, Vitest, Playwright

## Quick Start

1. Complete Workshop 1 first if you're new to Claude Code
2. Start with [cc-workshop-1/00_setup.md](./cc-workshop-1/00_setup.md)
3. After Workshop 2, try [Assignment 1](./cc-assignment-1/) to reinforce your learning

## Help

- Type `/help` inside Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
