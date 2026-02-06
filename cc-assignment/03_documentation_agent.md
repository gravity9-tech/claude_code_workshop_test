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

- Creates a `docs/` folder structure for storing project documentation (if it doesn't exist)
- Accepts implementation details as input
- Reads the changed code and generates appropriate documentation
- Keeps documentation in sync with new features
- Generates a changelog entry
- Only documents new or changed code

---

## Congratulations!

You've completed all assignments. Your development lifecycle now includes:

- Security review (Assignment 1)
- Full Jira-to-Done automation (Assignment 2)
- Auto-documentation (Assignment 3)

```
┌─────────────────────────────────────────────────────────────┐
│              Your Complete Development System               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  /full-lifecycle                                            │
│  ├── create-ticket (Jira)                                   │
│  ├── expand-ticket (Jira)                                   │
│  ├── planner (Agent)                                        │
│  ├── tdd-guide (Agent)                                      │
│  ├── code-reviewer (Agent)                                  │
│  ├── security-reviewer (Agent)                              │
│  ├── documentation-agent (Agent)                            │
│  └── implement-ticket (Jira → Done)                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

From idea to documented, tested, reviewed, secure code — all automated.
