# Workshop 04: Understanding Skills

**Duration: ~5 minutes**

## What You'll Learn

- What skills are and how they differ from tools
- How skills guide Claude to use tools effectively

---

## Tools vs Skills

In the previous module, you set up Jira MCP which gave Claude access to **tools** like `jira_create_issue` and `jira_search`. Tools are low-level operations — they do one thing.

**Skills** are markdown files that teach Claude *how* to complete specific tasks. They provide procedural knowledge — instructions, best practices, and templates that guide Claude's decision-making:

| | Tools (MCP) | Skills |
|---|-------------|--------|
| **What they are** | API connections | Markdown instructions |
| **What they provide** | Capabilities (actions) | Knowledge (how-to) |
| **Example** | `jira_create_issue` | `create-ticket` |
| **Scope** | Single action | Multi-step guidance |
| **Role** | Execute operations | Guide Claude's approach |

**Key insight:** MCP *provides* the tools; Skills teach Claude *how to use them effectively*.

---

## How Skills Guide Claude

A skill like `create-ticket` provides instructions that guide Claude:

```
You: "Create a ticket for a wishlist feature"
                    │
                    ▼
         ┌─────────────────────────┐
         │    create-ticket skill   │
         │                          │
         │  SKILL.md instructions:  │
         │  - Check for duplicates  │
         │  - Use BDD format        │
         │  - Include acceptance    │
         │    criteria              │
         └─────────────────────────┘
                    │
                    ▼
         ┌─────────────────────────┐
         │        Claude            │
         │                          │
         │  Follows instructions    │
         │  and decides to use:     │
         │  - jira_search (MCP)     │ ← Check duplicates
         │  - Read (built-in)       │ ← Get project context
         │  - jira_create_issue     │ ← Create ticket
         └─────────────────────────┘
```

Without the skill, you'd need to prompt Claude with all these instructions manually.

---

## Skill Structure

Every skill is a folder containing a `SKILL.md` file:

```
.claude/skills/
└── my-skill/
    └── SKILL.md
```

The `SKILL.md` file has two parts:

1. **YAML frontmatter** — metadata (name, description)
2. **Markdown body** — instructions Claude follows

```yaml
---
name: my-skill
description: Does something useful. Use when the user asks for X.
---

# Instructions

When this skill is active, follow these steps:
1. First, do this...
2. Then, do that...
```

---

## Where Skills Live

| Location | Path | Scope |
|----------|------|-------|
| Personal | `~/.claude/skills/` | All your projects |
| Project | `.claude/skills/` | Shared with team via git |

---

## Next Up

Continue to: [05_dev_lifecycle.md](./05_qa_lifecycle.md) - Build Your Dev Lifecycle Skill Suite
