# Skills for Reusable Workflows

Skills are on-demand instruction sets. They load only when invoked, keeping your base context clean.

## When to Use Skills vs Rules

| Use rules/ for | Use skills for |
|---------------|---------------|
| Conventions (always or conditionally) | Workflows (step-by-step processes) |
| Passive guidance | Active procedures |
| "How to write code" | "How to deploy/review/commit" |

## Structure

```
.claude/skills/
├── deploy/
│   └── SKILL.md
├── review/
│   └── SKILL.md
└── migrate/
    ├── SKILL.md
    └── templates/
        └── migration.sql
```

## Example Skill

```markdown
---
name: code-review
description: Reviews code against project standards
---

# Code Review Checklist
1. Check for type safety — no `any` usage
2. Verify error handling follows @src/middleware/errorHandler.ts pattern
3. Confirm tests exist and mirror src/ structure
4. Validate naming conventions match existing codebase
5. Check for duplicate utilities in @src/utils/
```

## Invocation

- Manual: `/code-review` in your prompt
- Automatic: Claude invokes when it detects relevance (unless disabled with `disable-model-invocation: true`)

## Skills as Context Engineering

Skills reduce the Task Layer burden. Instead of writing detailed instructions each time you want a code review, you invoke a skill. The knowledge lives in the system, not in your prompt.
