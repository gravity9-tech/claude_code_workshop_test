# Quick Wins — 15 Minutes to Better AI

You don't need a full Context Engineering setup to see results. Start here.

## Minute 0-5: Create CLAUDE.md

In your project root, create `CLAUDE.md` with:

```markdown
# [Project Name]

## Stack
[Language, framework, database — one line]

## Structure
- src/[key-dir]/ — [what lives here]
- [repeat for 3-5 key directories]

## Conventions
- [2-3 rules that differ from generic best practices]

## Commands
- [test command]
- [lint command]
- [build command]
```

That's it. This alone eliminates most drift.

## Minute 5-10: Add Output Rules

Append to CLAUDE.md:

```markdown
## Output
- Keep responses concise. Every line earns its place.
- Lead with the answer.
```

This reduces Response token waste immediately.

## Minute 10-15: Add One Rule File

Create `.claude/rules/` with one file for your most common pain point:

```bash
mkdir -p .claude/rules
```

Example — `.claude/rules/testing.md`:
```markdown
---
paths:
  - "**/*.test.*"
  - "**/*.spec.*"
---

# Testing Conventions
- Use [your test framework] with [your assertion style]
- Test files mirror src/ structure
- One describe block per exported function
```

## What You've Built

In 15 minutes:
- **Instruction layer** — CLAUDE.md (prevents drift)
- **Output discipline** — concise response rules (reduces drain)
- **Conditional layer** — one scoped rule (context efficiency)

This handles 80% of Token Drain & Drift for most projects.
