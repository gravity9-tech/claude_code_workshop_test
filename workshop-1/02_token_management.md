# Workshop 02: Token Management

**Duration: ~15 minutes**

## What You'll Learn

- How to monitor token usage in real time
- When and how to clear, compact, and start fresh
- Cost-conscious prompting patterns that save tokens

---

## Why Tokens Run Out

Most developers hit token limits because of three patterns:

1. **Kitchen sink sessions** — switching between unrelated tasks without clearing
2. **Unfocused exploration** — asking Claude to "look at the whole codebase"
3. **Correction loops** — fixing the same mistake repeatedly, polluting context with failed attempts

All three are avoidable.

---

## Monitoring Tools

### `/context` — What's in your window

```bash
/context
```

Shows exactly what's consuming your context: messages, file contents, tool results. Use this to identify what's taking up space.

### `/stats` — Session cost and usage

```bash
/stats
```

Shows tokens used and estimated cost for the current session.

---

## Task 1: Practice Clearing Context

Start a conversation and ask Claude a few questions about your project. Then:

```bash
/clear
```

Run `/context` again. The window is empty — Claude remembers nothing from before.

**When to use `/clear`:**
- Switching to an unrelated task
- Context is cluttered with irrelevant information
- You want a clean start

**Rule of thumb:** If your next question has nothing to do with what you were just doing, `/clear` first.

---

## Task 2: Practice Compacting

Start a new conversation. Ask Claude to explore a few files and explain what they do. Once you've had 5-6 exchanges, run:

```bash
/compact
```

Check `/context` — the conversation has been summarized, freeing up space while keeping key information.

You can also guide the compaction:

```bash
/compact Focus on the API changes we discussed
```

This tells Claude what to prioritize when summarizing.

---

## Task 3: Practice Rewinding

Ask Claude to make a change you know is wrong (or ask a question that leads nowhere). Then:

```bash
/rewind
```

Select a checkpoint to restore to. This rolls back the conversation **and** any code changes Claude made. Cleaner than saying "undo that" — it actually removes the failed attempt from context.

---

## Cost-Conscious Prompting Patterns

### Be specific about files

| Token-expensive | Token-efficient |
|----------------|-----------------|
| "Look at the whole src directory" | "Read src/auth/login.ts" |
| "Find all the tests" | "Read tests/auth.test.ts" |
| "What does this project do?" | "Read the README and summarize in 2 sentences" |

### Scope your requests

| Broad (fills context) | Scoped (preserves context) |
|----------------------|---------------------------|
| "Review this codebase" | "Review src/api/users.ts for SQL injection" |
| "Fix the tests" | "Fix the failing test in auth.test.ts line 42" |
| "Explain the architecture" | "How does request auth work? Check src/middleware/" |

### Clear between tasks

```
Task 1: Fix the login bug → /clear → Task 2: Add the search feature
```

Don't carry task 1's context into task 2. It wastes tokens and confuses Claude.

---

## When Context Gets Full

Claude Code automatically compacts when context is nearly full. But don't wait for that — proactive management gives better results:

| Context Usage | Action |
|--------------|--------|
| < 50% | Keep working |
| 50-75% | Consider `/compact` if switching tasks |
| > 75% | `/compact` or `/clear` before continuing |
| Auto-compact triggers | Context was too full — start managing earlier next time |

---

## Key Takeaways

- **Monitor** with `/context` and `/stats` regularly
- **Clear** between unrelated tasks
- **Compact** when staying on the same task but context is growing
- **Rewind** to remove failed attempts cleanly
- **Be specific** in prompts to avoid unnecessary file reads

---

Continue to: [03_core_workflows.md](./03_core_workflows.md)
