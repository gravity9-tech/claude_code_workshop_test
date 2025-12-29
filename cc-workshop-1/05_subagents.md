# Workshop 05: Introduction to Subagents

**Duration: ~10 minutes**

## What You'll Learn

- What subagents are and when to use them
- Built-in agents (Explore, Plan)
- Run agents in parallel

---

## What Are Subagents?

Subagents are autonomous Claude instances that:
- Work independently on tasks
- Can run in parallel
- Have specific tools and focus
- Report back results

---

## Built-in Agents

| Agent | Purpose |
|-------|---------|
| `Explore` | Find files, understand architecture |
| `Plan` | Design implementation approaches |

---

**Locations:**
- Project: `.claude/agents/`
- Personal: `~/.claude/agents/`

---

## Task 1: Using Built-in Agents

Try these prompts on the workshop codebase:

```
Explore the FastAPI application structure and explain how routes connect to mock_data
```

```
Find all files related to product filtering and price validation
```

```
Plan how to add a wishlist endpoint to app/api/routes.py
```

---

## Task 2: Create the Agents Directory

```bash
mkdir -p .claude/agents
```

---

## Task 3: Running Agents in Parallel (Within Claude)

Run multiple agents simultaneously in a single prompt:

```
Run these tasks in parallel:
1. Explore the codebase structure and list all Python files
2. Review app/mock_data.py for any hardcoded values that should be configurable
```

Claude spawns separate agents that work simultaneously and report back.

---

## Task 4: Running Agents in Separate Terminals

For long-running or independent tasks, use multiple terminal windows:

**Terminal 1 - Exploration:**
```bash
claude -p "Explore and document the API structure in app/api/routes.py"
```

**Terminal 2 - Review:**
```bash
claude -p "Review app/models.py for code quality issues"
```

**Terminal 3 - Documentation:**
```bash
claude -p "Generate docs for this codebase using component mermaid format"
```

| Approach | Best For |
|----------|----------|
| `Run in parallel...` (within Claude) | Quick coordinated tasks, shared context |
| Separate terminals with `-p` | Long tasks, different focus areas |

---

## Your Structure

```
.claude/
├── commands/
│   └── test-basket.md
└── agents/
    └── (custom agents go here)
```

---

## Checkpoint

- [ ] Used the Explore agent
- [ ] Used the Plan agent
- [ ] Created the agents directory
- [ ] Ran agents in parallel within Claude
- [ ] Tried running Claude in separate terminals

---

## Next Up

Continue to Workshop 2 for advanced topics: Skills, Custom Agents, and Hooks
