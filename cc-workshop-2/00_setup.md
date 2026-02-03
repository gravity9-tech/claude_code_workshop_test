# Workshop 00: Setup & Prerequisites

**Duration: ~5 minutes**

## What You'll Learn

- Verify Workshop 1 completion
- Understand what Workshop 2 will build
- Preview the subagents and hooks architecture

---

## Prerequisites

Before starting Workshop 2, you must have completed **Workshop 1** with:

- [ ] tea-store-demo project cloned and configured
- [ ] Jira MCP server configured and working
- [ ] Four lifecycle skills created:
  - `create-ticket`
  - `expand-ticket`
  - `implement-ticket`
  - `qa-ticket`
- [ ] `/deploy-ticket` slash command created
- [ ] Meta-skills available:
  - `skill-creator`
  - `command-creator`
  - `agent-creator` (for Workshop 2)

---

## Verification Steps

### 1. Check Skills Are Loaded

```
/skills
```

You should see:
- `skill-creator`
- `command-creator`
- `agent-creator`
- `create-ticket`
- `expand-ticket`
- `implement-ticket`
- `qa-ticket`

### 2. Check Command Exists

```bash
# macOS/Linux
cat .claude/commands/deploy-ticket.md

# Windows
type .claude\commands\deploy-ticket.md
```

Or open the file in your editor. Verify it has:
- YAML frontmatter with `description` and `allowed-tools`
- Workflow phases for create → expand → implement → qa

### 3. Check Jira MCP

```
/mcp
```

Verify Jira MCP server is connected and tools are available.

---

## What's New in Workshop 2

Workshop 2 builds on your existing skills by introducing three new concepts:

### 1. Subagents

Specialized agents that run in **isolated context windows**:
- Don't pollute your main conversation context
- Can run in **parallel** for faster execution
- Return only **summarized results**

### 2. Skill Injection

Take your existing Workshop 1 skills and **inject them into agents**:
- `dev-agent` gets: create-ticket + expand-ticket + implement-ticket
- `qa-agent` gets: qa-ticket

Skills remain the source of truth. Agents provide isolated execution.

### 3. Hooks

Event-driven automation that triggers on Claude Code actions:
- `PreToolUse` - Run before a tool executes
- `PostToolUse` - Run after a tool completes
- Enable validation, logging, notifications

---

## Workshop 2 Architecture Preview

```
┌─────────────────────────────────────────────────────────────┐
│                    WORKSHOP 2 STACK                          │
│                                                              │
│   HOOKS ──────────► Event-driven triggers                   │
│       │              (pre/post tool execution)              │
│       ▼                                                      │
│   SLASH COMMAND ──► /deploy-ticket                          │
│       │              (updated for agent orchestration)      │
│       ▼                                                      │
│   SUBAGENTS ──────► dev-agent, qa-agent                     │
│       │              (parallel execution, isolated context) │
│       ▼                                                      │
│   SKILLS ─────────► create-ticket, expand-ticket, etc.      │
│                      (from Workshop 1, injected into agents) │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## The Journey: Workshop 1 → Workshop 2

| Component | Workshop 1 | Workshop 2 |
|-----------|-----------|------------|
| Skills | Created 4 lifecycle skills | Same skills, injected into agents |
| Agents | N/A | dev-agent, qa-agent |
| Commands | Sequential skill execution | Parallel agent orchestration |
| Hooks | N/A | Pre/post automation |
| Context | Main window fills up | Isolated per agent |
| Execution | Sequential | Parallel |

---

## Checkpoint

Before proceeding, confirm:

- [ ] All Workshop 1 skills are loaded
- [ ] `/deploy-ticket` command exists
- [ ] Jira MCP is connected
- [ ] You understand the Workshop 2 architecture

---

## Next Up

Continue to [01_subagents_intro.md](./01_subagents_intro.md) to learn about subagents and how Claude delegates tasks.
