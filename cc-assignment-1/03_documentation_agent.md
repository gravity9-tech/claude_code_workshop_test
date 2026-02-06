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

## Part 1: Create the Agent

Create the `documentation-agent` based on the requirements above.

Verify it exists at `.claude/agents/documentation-agent.md`

---

## Part 2: Test in Isolation

Test the agent on some recently changed code in your project. Verify it:
- Reads the code correctly
- Generates appropriate documentation
- Follows project conventions

---

## Part 3: Integration (Optional)

Choose one:

**Option A:** Add to `/orchestrate` as a fifth phase
```
planner → tdd-guide → code-reviewer → security-reviewer → documentation-agent
```

**Option B:** Create a standalone `/document` command that can be run independently on any files.

---

## Success Criteria

- [ ] `documentation-agent` exists in `.claude/agents/`
- [ ] Agent can read code and generate appropriate docs
- [ ] Generated docs follow project conventions
- [ ] Changelog entries are properly formatted
- [ ] (Optional) Integrated into workflow or available as standalone command

---

## Next Assignment

Continue to: [04_git_commits.md](./04_git_commits.md) — Automatic Git Commits (Bonus)
