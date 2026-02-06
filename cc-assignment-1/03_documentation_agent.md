# Assignment 03: Documentation Agent (Bonus)

**Estimated Duration: 15-20 minutes**

## Objective

Create a `documentation-agent` that reads implementation changes and generates or updates documentation (README sections, API docs, JSDoc/docstrings).

---

## Background

After code is implemented and reviewed, documentation often gets neglected. This agent automates documentation generation based on what was actually built.

```
code-reviewer → documentation-agent
                      │
                      ▼
              ┌───────────────────┐
              │ - README updates  │
              │ - API docs        │
              │ - Code comments   │
              └───────────────────┘
```

---

## Requirements

Create a `documentation-agent` that:

- Accepts implementation details as input
- Reads the changed code and generates appropriate documentation
- Follows existing documentation conventions in the project
- Generates a changelog entry
- Only documents new or changed code

---

## Next Assignment

Continue to: [04_git_commits.md](./04_git_commits.md) — Automatic Git Commits (Bonus)
