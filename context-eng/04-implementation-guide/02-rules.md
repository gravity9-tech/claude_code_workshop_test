# Structuring Rules

Rules are the conditional layer. They load when relevant, keeping your base context lean.

## Where They Live

```
.claude/
└── rules/
    ├── code-style.md        # Always loaded
    ├── testing.md           # Always loaded
    ├── frontend/
    │   └── react.md         # Path-scoped to src/components/**
    └── backend/
        └── api.md           # Path-scoped to src/api/**
```

## Unconditional vs Path-Scoped

**Unconditional** — no frontmatter, loads every session:
```markdown
# Error Handling
- Always use custom AppError class
- Never expose stack traces in API responses
```

**Path-scoped** — loads only when matching files are touched:
```markdown
---
paths:
  - "src/components/**/*.tsx"
---

# React Conventions
- Use functional components only
- State management via useReducer for complex state
- Props interfaces named {ComponentName}Props
```

## When to Use Rules vs CLAUDE.md

| Use CLAUDE.md for | Use rules/ for |
|-------------------|---------------|
| Architecture overview | Domain-specific conventions |
| Build commands | File-type-specific patterns |
| Universal conventions | Team/module boundaries |
| Project-wide "never do X" | Conditional instructions |

## Keep Them Focused

Each rule file should cover one topic. If a rule file exceeds 50 lines, it's probably doing too much. Split it.

## Sharing Across Projects

Symlink shared rules:
```bash
ln -s ~/company-standards/security.md .claude/rules/security.md
```
