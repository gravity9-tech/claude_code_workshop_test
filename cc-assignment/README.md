# Claude Code Assignment: Extending the Development Lifecycle

**Estimated Duration: 60-90 minutes**

These assignments reinforce concepts from Workshop 1 and Workshop 2 by challenging you to extend the development lifecycle automation you've built.

## Prerequisites

- Completed [Workshop 1](../cc-workshop-1/) (Jira MCP, skills, `/deploy-ticket`)
- Completed [Workshop 2](../cc-workshop-2/) (agents, `/orchestrate`)

## Assignments

| # | Assignment | Description | Type |
|---|------------|-------------|------|
| 01 | [Security Reviewer](./01_security_reviewer.md) | Add a security review phase to `/orchestrate` | Required |
| 02 | [Full Lifecycle](./02_full_lifecycle.md) | Bridge Jira skills (W1) with agents (W2) | Required |
| 03 | [Documentation Agent](./03_documentation_agent.md) | Auto-generate documentation from code | Bonus |

## What You'll Practice

```
Workshop 1 Concepts          Workshop 2 Concepts
────────────────────         ────────────────────
- Jira MCP tools             - Custom agents
- Skills (knowledge)         - Skill injection
- /deploy-ticket command     - /orchestrate command

                    │
                    ▼
         ┌─────────────────────┐
         │   Assignment Work    │
         │                      │
         │  - Extend agents     │
         │  - Create commands   │
         │  - Bridge workshops  │
         └─────────────────────┘
```

## Tips for Success

1. **Read existing artifacts first** — Before creating something new, review how similar artifacts are structured in your `.claude/skills/`, `.claude/agents/`, and `.claude/commands/` folders.

2. **Test incrementally** — After creating each artifact, test it in isolation before integrating it into a larger workflow.

3. **Check `/mcp`** — If Jira operations fail, verify your MCP server is connected.

---

Start with [01_security_reviewer.md](./01_security_reviewer.md)
