# Claude Code Workshop 2: Subagents & Orchestration

**Total Duration: ~70 min**

Enhance your Workshop 1 skills by injecting them into subagents for parallel execution, updating your slash command for agent orchestration, and adding event-driven hooks.

## Prerequisites

Before starting Workshop 2, you must have completed **Workshop 1** with:

- tea-store-demo project configured
- Four lifecycle skills: `create-ticket`, `expand-ticket`, `implement-ticket`, `qa-ticket`
- `/deploy-ticket` slash command
- Jira MCP server configured
- Meta-skills: `skill-creator`, `command-creator`, `agent-creator`

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 00 | [Setup](./00_setup.md) | 5 min |
| 01 | [Subagents Introduction](./01_subagents_intro.md) | 10 min |
| 02 | [Custom Subagents](./02_custom_subagents.md) | 12 min |
| 03 | [Skills + Agents](./03_skills_and_agents.md) | 15 min |
| 04 | [Orchestrating Agents](./04_orchestrating_agents.md) | 15 min |
| 05 | [Hooks](./05_hooks.md) | 15 min |
| 06 | [Full Workflow](./06_full_workflow.md) | 15 min |

---

## Learning Path

```
Workshop 1 Complete
(skills + /deploy-ticket)
         │
         ▼
00 Setup (verify prerequisites)
         │
         ▼
01 Subagents Intro (built-in agents, Task tool)
         │
         ▼
02 Custom Subagents (create AGENT.md structure)
   └── code-reviewer agent (standalone)
         │
         ▼
03 Skills + Agents (INJECT existing skills)
   ├── dev-agent ← create + expand + implement skills
   └── qa-agent ← qa-ticket skill
         │
         ▼
04 Orchestrating Agents (UPDATE /deploy-ticket)
   └── Parallel agent execution
         │
         ▼
05 Hooks (event-driven automation)
   └── Pre/post tool hooks
         │
         ▼
06 Full Workflow (complete demonstration)
```

---

## Key Concept: Evolution, Not Recreation

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│   Workshop 1 created:          Workshop 2 enhances:         │
│                                                              │
│   • create-ticket skill   ──►  Injected into dev-agent      │
│   • expand-ticket skill   ──►  Injected into dev-agent      │
│   • implement-ticket skill──►  Injected into dev-agent      │
│   • qa-ticket skill       ──►  Injected into qa-agent       │
│   • /deploy-ticket        ──►  Updated for parallel agents  │
│                                                              │
│   Skills remain the source of truth.                        │
│   Agents provide isolated execution.                        │
│   Command orchestrates everything.                          │
│   Hooks add automation.                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## What You'll Learn

| Module | Concept | What It Does |
|--------|---------|--------------|
| 01 | Subagents | Isolated context, parallel execution |
| 02 | AGENT.md | Custom agent structure and frontmatter |
| 03 | Skill Injection | Inject existing skills into agents |
| 04 | Orchestration | Update command for parallel agents |
| 05 | Hooks | Event-driven pre/post automation |
| 06 | Full Workflow | Complete end-to-end integration |

---

## Final Project Structure

After completing Workshop 2:

```
tea-store-demo/
├── .claude/
│   ├── agents/                   # NEW in Workshop 2
│   │   ├── code-reviewer.md      # Standalone agent
│   │   ├── dev-agent.md          # Injects 3 skills
│   │   └── qa-agent.md           # Injects 1 skill
│   ├── commands/
│   │   └── deploy-ticket.md      # UPDATED for agents
│   ├── hooks/                    # NEW in Workshop 2
│   │   ├── pre-commit-check.sh
│   │   ├── log-activity.sh
│   │   └── notify-complete.sh
│   ├── skills/                   # FROM Workshop 1 + agent-creator
│   │   ├── skill-creator/        # Meta-skill
│   │   ├── command-creator/      # Meta-skill
│   │   ├── agent-creator/        # Meta-skill (used in W2)
│   │   ├── create-ticket/        # → dev-agent
│   │   ├── expand-ticket/        # → dev-agent
│   │   ├── implement-ticket/     # → dev-agent
│   │   └── qa-ticket/            # → qa-agent
│   └── settings.json             # NEW: hook config
└── ...
```

---

## What You'll Accomplish

By the end of Workshop 2:

1. **Understand subagent architecture** — isolated context, parallel execution
2. **Create custom agents** — AGENT.md files with frontmatter
3. **Inject existing skills** — dev-agent and qa-agent with Workshop 1 skills
4. **Update orchestration** — /deploy-ticket runs agents in parallel
5. **Implement hooks** — event-driven automation
6. **Run complete workflow** — end-to-end with all components

---

## Before & After Comparison

| Aspect | Workshop 1 | Workshop 2 |
|--------|-----------|------------|
| **Execution** | Sequential | Parallel |
| **Context** | Main window fills | Isolated per agent |
| **Skills** | Direct calls | Injected into agents |
| **Automation** | Manual trigger only | Hooks for events |
| **Speed** | Sum of all steps | Longest step only |

---

## Troubleshooting

**Agent not loading:**
- Check file is in `.claude/agents/`
- Verify `name` field in frontmatter
- Restart Claude Code

**Skill not injected:**
- Check `skills:` list in frontmatter
- Verify skill file exists

**Hooks not firing:**
- Check matcher pattern in settings.json
- macOS/Linux: Ensure scripts are executable (`chmod +x`)
- Windows: Use Git Bash or WSL to run bash hook scripts
- Check hook script exit codes

**Parallel execution not working:**
- Ensure agents are independent
- Check that command instructs parallel launch

---

## Help

- Type `/help` in Claude Code
- Type `/agents` to see available agents
- Type `/skills` to see available skills
- [Claude Code Docs](https://docs.anthropic.com/claude-code)

---

## Start Here

Begin with [00_setup.md](./00_setup.md) to verify your Workshop 1 completion and preview Workshop 2.
