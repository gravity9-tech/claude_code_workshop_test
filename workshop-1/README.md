# Claude Code Workshop 1: Getting Productive

**Total Duration: ~2 hours**

A hands-on workshop for developers learning Claude Code. You'll go from understanding the basics to building a custom skill that automates your development workflow with Jira.

## Prerequisites

- Claude Code installed and authenticated
- `gh` CLI installed and authenticated (`gh auth login`)
- Node.js 22+ (for MCP servers)
- Jira Cloud account with project access
- A project repository to work in

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 01 | [Understanding the Context Window](./01_context_window.md) | 10 min |
| 02 | [Token Management](./02_token_management.md) | 15 min |
| 03 | [Core Workflows](./03_core_workflows.md) | 15 min |
| 04 | [CLAUDE.md & Memory](./04_claude_md.md) | 15 min |
| 05 | [Prompting Best Practices](./05_prompting.md) | 10 min |
| 06 | [Git & PR Workflows](./06_git_workflows.md) | 15 min |
| 07 | [MCP Servers](./07_mcp_servers.md) | 15 min |
| 08 | [Skills](./08_skills.md) | 15 min |
| 09 | [Full Workflow Exercise](./09_full_workflow.md) | 10 min |

## Learning Path

```
Context Window → Token Management → Core Workflows
                                         │
                                         ▼
                                  ┌─────────────┐
                                  │  CLAUDE.md   │  ← Project instructions
                                  └──────┬──────┘
                                         │
                                         ▼
                                  ┌─────────────┐
                                  │  Prompting   │  ← Get better results
                                  └──────┬──────┘
                                         │
                          ┌──────────────┼──────────────┐
                          ▼              ▼              ▼
                   ┌────────────┐ ┌───────────┐ ┌───────────┐
                   │    Git &   │ │    MCP    │ │  Skills   │
                   │     PRs    │ │  Servers  │ │           │
                   └─────┬──────┘ └─────┬─────┘ └─────┬─────┘
                         │              │              │
                         └──────────────┼──────────────┘
                                        ▼
                               ┌─────────────────┐
                               │  Full Workflow   │  ← Capstone exercise
                               │  /fix-issue      │
                               └─────────────────┘
```

## What You'll Build

By the end of this workshop you will have:

- A `CLAUDE.md` with project-specific instructions
- Filesystem and Jira MCP servers connected
- A custom `/fix-issue` skill that reads a Jira ticket and automates implementation
- A PR created entirely through Claude Code

---

Start with [01_context_window.md](./01_context_window.md)
