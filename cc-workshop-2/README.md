# Claude Code Workshop 2: Advanced Features

**Total Duration: ~58 min**

Advanced Claude Code features: Skills, Custom Agents, Hooks, Marketplace, Plugins, and Workflow Automation.

## Prerequisites

- Completed Workshop 1 (or equivalent knowledge)
- Claude Code installed (`npm install -g @anthropic-ai/claude-code`)
- Familiar with slash commands and basic subagents

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 01 | [Skills](./01_skills.md) | 12 min |
| 02 | [Subagents with Skills](./02_subagents.md) | 16 min |
| 03 | [Hooks](./03_hooks.md) | 8 min |
| 04 | [Marketplace](./04_marketplace.md) | 8 min |
| 05 | [Plugins](./05_plugins.md) | 8 min |
| 06 | [Workflow Automation](./06_workflow_automation.md) | 10 min |

## Learning Path

```
01 Skills (domain expertise)
         |
         v
02 Subagents with Skills (agents that use skills)
         |
         v
03 Hooks (event automation)
         |
         v
04 Marketplace (community resources)
         |
         v
05 Plugins (create & share)
         |
         v
06 Workflow Automation (CI/CD)
```

---

## What You'll Learn

| Module | Feature | What It Does |
|--------|---------|--------------|
| 01 | Skills | Domain expertise for Claude |
| 02 | Agents + Skills | Custom agents that use skills |
| 03 | Hooks | Shell commands on events |
| 04 | Marketplace | Community resources & sharing |
| 05 | Plugins | Package & distribute extensions |
| 06 | Workflow Automation | Multi-step command workflows |

---

## Final Project Structure

After completing Workshop 2:

```
your-project/
├── .claude/
│   ├── commands/
│   │   └── test-basket.md
│   ├── skills/
│   │   └── reviewing-code/
│   │       ├── SKILL.md
│   │       └── reference/
│   │           ├── security-checklist.md
│   │           ├── quality-checklist.md
│   │           └── output-format.md
│   ├── agents/
│   │   └── reviewing-code.md
│   └── settings.json            # Hooks config
├── .mcp.json
└── CLAUDE.md
```

---

## Troubleshooting

**Skills not loading:**
```bash
ls .claude/skills/*/SKILL.md
```

**Agents not found:**
```bash
ls .claude/agents/
```

**Hooks not running:**
```bash
cat .claude/settings.json
```

## Help

- Type `/help` in Claude Code
- [Claude Code Docs](https://docs.anthropic.com/claude-code)

---

Start with [01_skills.md](./01_skills.md)
