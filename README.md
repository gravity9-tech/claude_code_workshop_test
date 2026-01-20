# Claude Code Workshops

Hands-on workshops for learning Claude Code and ticket-driven development with the dev-tools plugin.

## Getting Started

### Prerequisites

- **Python 3.12+** for the backend
- **Node.js 18+** for the frontend and Claude Code
- Claude Code installed:
  ```bash
  npm install -g @anthropic-ai/claude-code
  ```
- Jira Cloud account with API access
- Basic command line familiarity

### Installation

```bash
git clone https://github.com/gravity9-tech/workshops.git
cd workshops
```

## Workshops

| Workshop | Level | Duration | Description |
|----------|-------|----------|-------------|
| [Workshop 1](./cc-workshop-1/) | Foundations | ~50 min | Setup, Jira MCP, skills, create & plan tickets |
| [Workshop 2](./cc-workshop-2/) | Advanced | ~60 min | Playwright, agents, implement & QA tickets, full workflow |

## Learning Path

```
Workshop 1: Foundations (Jira workflow)
├── 00 Project Setup (pandora-demo)
├── 01 Context Window
├── 02 Getting Started (/init, CLAUDE.md)
├── 03 Jira MCP Setup
├── 04 Skills Introduction
├── 05 Create Ticket Skill
└── 06 Plan Ticket Skill
         │
         │  [You now have a planned Jira ticket]
         │
         v
Workshop 2: Advanced (Build & Test)
├── 01 Playwright MCP Setup
├── 02 Slash Commands
├── 03 Custom Agents
├── 04 Implement Ticket Skill
├── 05 QA Ticket Skill
├── 06 Hooks
└── 07 Full Workflow (/build-and-qa)
```

## What You'll Build

By completing both workshops, you'll:

1. **Set up Claude Code** with a real project (pandora-demo)
2. **Connect to Jira** via MCP for ticket management
3. **Create tickets** with BDD acceptance criteria
4. **Plan tickets** with phase subtasks (P1/P2/QA)
5. **Implement features** phase-by-phase
6. **Run automated QA** with Playwright browser testing
7. **Use the full workflow** `/build-and-qa` command

## Target Project

Both workshops use [pandora-demo](https://github.com/gravity9-tech/pandora-demo), a full-stack luxury jewelry e-commerce application:

- **Backend**: FastAPI, Python 3.12+, Pydantic
- **Frontend**: Angular 19, TypeScript, Tailwind CSS 4
- **Features**: Product filtering, cart, wishlist, customization, dark mode

## Quick Start

1. Complete Workshop 1 first if you're new to Claude Code
2. Start with [cc-workshop-1/00_setup.md](./cc-workshop-1/00_setup.md)

## Help

- Type `/help` inside Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
