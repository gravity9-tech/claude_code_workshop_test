# Claude Code Workshop 2: Automation & Scaling

**Total Duration: ~85 minutes**

A hands-on workshop for developers ready to go beyond the basics. You'll learn to automate workflows with hooks, build custom agents, and run parallel sessions at scale.

## Prerequisites

- **Workshop 1 completed** (context window, CLAUDE.md, MCP, skills)
- Claude Code installed and authenticated
- `gh` CLI installed and authenticated (`gh auth login`)
- Node.js 18+ (for MCP servers)
- A project repository to work in (the Tea Store from Workshop 1)

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 01 | [Permissions & Security](./01_permissions_security.md) | 15 min |
| 02 | [Hooks](./02_hooks.md) | 20 min |
| 03 | [Custom Agents](./03_custom_agents.md) | 20 min |
| 04 | [Parallel Workflows & Scaling](./04_parallel_workflows.md) | 15 min |
| 05 | [Full Workflow Exercise](./05_full_workflow.md) | 15 min |

## Learning Path

```
Permissions & Security ──► Hooks ──► Custom Agents
        │                    │              │
        │    Foundation      │   Automation │   Delegation
        │                    │              │
        └────────────────────┴──────┬───────┘
                                    │
                                    ▼
                           ┌──────────────────┐
                           │    Parallel       │
                           │   Workflows       │
                           └────────┬─────────┘
                                    │
                                    ▼
                           ┌─────────────────┐
                           │  Full Workflow   │  ← Capstone exercise
                           │  End-to-end      │
                           └─────────────────┘
```

## What You'll Build

By the end of this workshop you will have:

- Project-level permission rules in `.claude/settings.json`
- Hooks that auto-lint code and block dangerous commands
- Custom agents for testing and code review
- Experience running parallel Claude sessions with worktrees

## Your Project After This Workshop

```
your-project/
├── .claude/
│   ├── agents/
│   │   ├── test-runner.md        # Runs tests, reports results
│   │   └── reviewer.md           # Reviews code against conventions
│   ├── settings.json             # Permission rules & hooks
│   └── skills/                   # From Workshop 1
├── CLAUDE.md                     # Project instructions
└── ...
```

---

Start with [01_permissions_security.md](./01_permissions_security.md)
