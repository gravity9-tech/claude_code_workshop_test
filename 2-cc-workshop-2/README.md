# Claude Code Workshop 2: Development Lifecycle

**Total Duration: ~85 min**

A hands-on guide to building a complete development lifecycle automation using custom agents, skills, and slash commands in Claude Code.

## Prerequisites

- Completed [Workshop 1](../cc-workshop-1/)
- Claude Code installed (`npm install -g @anthropic-ai/claude-code`)
- Node.js (for MCP servers)
- Basic command line familiarity

## Workshop Modules

| Module | Topic | Duration |
|--------|-------|----------|
| 00 | [Setup](./00_setup.md) | 5 min |
| 01 | [Dev Lifecycle Introduction](./01_dev_lifecycle_intro.md) | 10 min |
| 02 | [TDD Workflow Skill](./02_tdd_workflow_skill.md) | 10 min |
| 03 | [Understanding Agents](./03_agents_intro.md) | 10 min |
| 04 | [Planner Agent](./04_planner_agent.md) | 10 min |
| 05 | [TDD Guide Agent](./05_tdd_guide_agent.md) | 10 min |
| 06 | [Code Reviewer Agent](./06_code_reviewer_agent.md) | 10 min |
| 07 | [Orchestrate Command](./07_orchestrate_command.md) | 10 min |
| 08 | [Full Workflow](./08_full_workflow.md) | 10 min |

## Learning Path

```
Setup → Concept Introduction
              │
              ▼
      ┌───────────────┐
      │  tdd-workflow  │  ← Skill (knowledge layer)
      │    (skill)     │
      └───────┬───────┘
              │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
┌────────┐ ┌─────────┐ ┌─────────────┐
│planner │ │tdd-guide│ │code-reviewer│  ← Agents (execution layer)
│        │ │(+skill) │ │             │
└───┬────┘ └───┬─────┘ └──────┬──────┘
    │          │              │
    └──────────┼──────────────┘
               ▼
      ┌────────────────┐
      │  /orchestrate   │  ← Command (orchestration layer)
      └───────┬────────┘
              ▼
        Full Workflow
```

---

## Final Project Structure

After completing Workshop 2:

```
tea-store-demo/
├── .claude/
│   ├── agents/
│   │   ├── planner.md          # Agent: breaks work into implementation steps
│   │   ├── tdd-guide.md        # Agent: TDD-driven implementation (+skill)
│   │   └── code-reviewer.md    # Agent: reviews code against plan & standards
│   ├── commands/
│   │   └── orchestrate.md      # Slash command: chains planner → tdd-guide → code-reviewer
│   └── skills/
│       ├── skill-creator/      # Pre-built: creates domain skills
│       ├── command-creator/    # Pre-built: creates slash commands
│       ├── agent-creator/      # Pre-built: creates custom agents
│       └── tdd-workflow/       # Your skill: TDD methodology knowledge
└── CLAUDE.md                    # Project memory (via /init)
```

---

## Troubleshooting

**Claude Code not found:**
```bash
npm install -g @anthropic-ai/claude-code
```

**Skills not triggering:**
- Ensure your task description matches the skill's trigger words
- Check that MCP servers are running with `/mcp`

**Agents not found:**
- Verify `.claude/agents/` directory exists
- Check agent file names match expected format

## Help

- Type `/help` in Claude Code
- Type `/mcp` to check MCP server status
- [Claude Code Docs](https://docs.anthropic.com/claude-code)

---

Start with [00_setup.md](./00_setup.md)
