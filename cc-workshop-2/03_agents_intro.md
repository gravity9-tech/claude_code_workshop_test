# Workshop 03: Understanding Agents

**Duration: ~10 minutes**

## What You'll Learn

- What agents are and how they differ from skills
- How agents run in isolated context windows
- The structure of an `AGENT.md` file
- How agents inject skills for additional knowledge

---

## Skills vs Agents

In Workshop 1 and the previous module, you built **skills** — markdown instructions that teach Claude how to do things. Skills are knowledge. But they run in the **main context window**, sharing space with everything else in your conversation.

**Agents** are isolated workers. Each agent gets its own context window, runs independently, and returns a result when done.

| | Skills | Agents |
|---|--------|--------|
| **What they are** | Knowledge files | Isolated executors |
| **Where they run** | Main context window | Their own context window |
| **Format** | `SKILL.md` | `AGENT.md` |
| **Location** | `.claude/skills/` | `.claude/agents/` |
| **Purpose** | Teach *how* to do something | *Do* something focused |
| **Reusability** | Injected into agents or main session | Invoked by commands or directly |

**Key insight:** Skills carry knowledge. Agents carry intent and execute in isolation.

---

## Why Isolation Matters

When Claude reads files, runs commands, and generates output in the main context window, all of that accumulates. A long task can fill the context with noise, making Claude lose track of earlier information.

Agents solve this:

```
┌─────────────────────────────────────────────┐
│          Main Context Window                │
│                                             │
│  You: "Run /orchestrate for feature X"      │
│                                             │
│  ┌──────────────┐  Result only              │
│  │ planner agent │ ──────────►  Plan text   │
│  │ (own context) │  returned to main        │
│  └──────────────┘                           │
│                                             │
│  All the files the agent read, the          │
│  commands it ran, its internal reasoning    │
│  — none of that pollutes the main context.  │
└─────────────────────────────────────────────┘
```

Each agent:
- Gets a **fresh context window** when it starts
- Can read files, run commands, and use tools independently
- Returns only its **final result** to the caller
- Its internal work is **discarded** after completion

This means you can chain multiple agents without the main context growing uncontrollably.

---

## Agent Structure

Every agent is a markdown file in `.claude/agents/`:

```
.claude/agents/
└── my-agent.md
```

The file has two parts:

1. **YAML frontmatter** — metadata (name, description, tools, model, skills)
2. **Markdown body** — instructions the agent follows

```yaml
---
name: my-agent
description: Does something specific. Use when the user asks for X.
tools: Read, Write, Edit, Bash, Glob, Grep
model: sonnet
skills:
  - some-skill
---

# My Agent

## Purpose
One sentence describing what this agent does.

## Process
1. First, do this...
2. Then, do that...
3. Finally, return the result.

## Output Format
Describe the expected output structure.
```

---

## Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Lowercase, hyphenated identifier |
| `description` | Yes | What it does and when to use it |
| `tools` | Yes | Comma-separated list of allowed tools |
| `model` | No | `sonnet`, `opus`, or `haiku` (omit to inherit) |
| `skills` | No | List of skill names to inject into the agent's context |

### Choosing Tools

Give agents only the tools they need:

| Agent Purpose | Recommended Tools |
|---------------|-------------------|
| Planning / analysis | Read, Glob, Grep |
| Implementation | Read, Write, Edit, Bash, Glob, Grep |
| Code review | Read, Glob, Grep, Bash |
| Research | Read, Glob, Grep, WebSearch, WebFetch |

### Choosing a Model

| Model | Best For |
|-------|----------|
| `opus` | Complex reasoning, architectural decisions |
| `sonnet` | Balanced — good default for most agents |
| `haiku` | Fast, simple tasks, lightweight checks |

---

## How Agents Inject Skills

An agent can list skills in its frontmatter. When the agent starts, those skills are loaded into its context automatically:

```
┌───────────────────────────────────────────┐
│            tdd-guide agent                │
│                                           │
│  Frontmatter:                             │
│    skills:                                │
│      - tdd-workflow                       │
│                                           │
│  ┌─────────────────────────────────┐      │
│  │  tdd-workflow SKILL.md content  │      │
│  │  is loaded into this agent's    │      │
│  │  context automatically          │      │
│  └─────────────────────────────────┘      │
│                                           │
│  The agent now has TDD knowledge          │
│  without us copying instructions          │
└───────────────────────────────────────────┘
```

This is why we built the `tdd-workflow` skill first — it exists as a reusable knowledge module that agents can pull in.

---

## Where Agents Live

| Location | Path | Scope |
|----------|------|-------|
| Personal | `~/.claude/agents/` | All your projects |
| Project | `.claude/agents/` | Shared with team via git |

For this workshop, we'll create agents in the project directory so they're part of the codebase.

---

## Next Up

Continue to: [04_planner_agent.md](./04_planner_agent.md) — Create the Planner Agent
