# Workshop 03: Custom Agents

**Duration: ~10 minutes**

## What You'll Learn

- What agents are and how they differ from commands
- How agents get their own context window
- Create a custom agent that uses skills
- Run agents in parallel

---

## What Are Agents?

**Agents** are autonomous Claude instances that:

- Work **independently** on tasks
- Have their **own context window** (don't fill up yours)
- Can **use skills** for domain expertise
- Return only a **summary** to your main context

---

## Agents vs Commands

| | Commands | Agents |
|---|----------|--------|
| **Context** | Runs in main context | Gets own context |
| **Invocation** | Explicit `/command` | Via "use agent" or parallel request |
| **Best for** | Quick, explicit tasks | Complex, autonomous work |
| **Output** | Full output in main | Summary only |

---

## Built-in Agents

Claude Code has built-in agents you've already used:

| Agent | Purpose |
|-------|---------|
| `Explore` | Find files, understand architecture |
| `Plan` | Design implementation approaches |

When you ask Claude to explore the codebase, it spawns an Explore agent with its own context.

---

## Agent Location

Custom agents live in:
- **Project:** `.claude/agents/`
- **Personal:** `~/.claude/agents/`

---

## Task 1: Create Agents Directory

```bash
mkdir -p .claude/agents
```

---

## Task 2: Create a Feature Builder Agent

This agent combines exploration and building capabilities.

Create `.claude/agents/feature-builder.md`:

```markdown
---
description: Builds features by analyzing codebase and implementing changes
skills: implement-ticket
allowed-tools: Read, Write, Edit, Grep, Glob, Bash
---

# Feature Builder Agent

You are a feature builder agent that implements features in a codebase.

## Process

1. **Understand the request** - What feature/change is needed?
2. **Analyze the codebase** - Find relevant files and patterns
3. **Plan the changes** - Identify what files need modification
4. **Implement** - Make the changes following existing patterns
5. **Verify** - Run tests if available

## Guidelines

- Follow existing code patterns and conventions
- Keep changes minimal and focused
- Add comments only where logic is non-obvious
- Run tests after making changes

## Output

Report:
- What files were changed
- What was added/modified
- Any issues encountered
```

---

## Task 3: Understand the Agent Structure

Let's break down the agent file:

```markdown
---
description: ...     # When to use this agent
skills: ...          # Skills the agent can access
allowed-tools: ...   # What tools it can use
---

# Instructions       # How the agent should behave
```

**Key field: `skills`**

The `skills: implement-ticket` line tells the agent to load the implement-ticket skill's instructions and reference files.

---

## Task 4: Run Agents in Parallel

You can run multiple agents simultaneously. Try this:

```
Run these tasks in parallel:
1. Explore the codebase and list all API endpoints
2. Find all files that handle product data
```

Claude spawns separate agents, each with their own context, working simultaneously.

---

## Task 5: Run Agent in Separate Terminal

For long-running tasks, use a separate terminal:

```bash
claude -p "Explore this codebase and document the API structure"
```

The `-p` flag runs Claude with a prompt and exits when done.

**Benefits:**
- Doesn't block your main session
- Gets its own full context
- Can run multiple in parallel (multiple terminals)

---

## How the Dev-Tools Plugin Uses Agents

The dev-tools plugin has internal agents for orchestration:

| Agent | Purpose |
|-------|---------|
| `ticket-creator` | Orchestrates ticket creation |
| `ticket-planner` | Orchestrates ticket planning |
| `ticket-implementer` | Orchestrates implementation |
| `qa-tester` | Orchestrates QA testing |

These agents are used internally by the `/build-and-qa` command.

---

## Context Efficiency

```
┌─────────────────────────────────────────────┐
│           Your Main Context                 │
│  ┌───────────────────────────────────────┐  │
│  │  Your conversation (small)            │  │
│  └───────────────────────────────────────┘  │
│                     │                       │
│              Agent Summary                  │
│              (just results)                 │
│                     ▲                       │
│  ┌─────────────────┴───────────────────┐   │
│  │                                      │   │
│  │         Agent Context               │   │
│  │  (full work, discarded after)       │   │
│  │                                      │   │
│  └──────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

Agents can read many files, do complex analysis, and only return a concise summary.

---

## Your Structure

```
.claude/
├── commands/
│   ├── test-feature.md
│   └── quick-check.md
└── agents/
    └── feature-builder.md     # Custom agent with skills
```

---

## Checkpoint

Before continuing, verify:

- [ ] Created `.claude/agents/` directory
- [ ] Created `feature-builder.md` agent
- [ ] Understand how agents use skills
- [ ] Tried running agents in parallel
- [ ] Understand context efficiency (agents have own context)

---

## What You've Learned

1. **Agents** are autonomous with their own context
2. **`skills:` field** binds skills to agents
3. **Parallel execution** via "Run in parallel" or multiple terminals
4. **Context efficiency** — only summaries return to main context

---

## Next Up

Now let's use a real skill to implement your ticket!

Continue to: [04_implement_ticket.md](./04_implement_ticket.md) - Build your feature
