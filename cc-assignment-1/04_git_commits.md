# Assignment 04: Automatic Git Commits (Bonus)

**Estimated Duration: 15-20 minutes**

## Objective

Modify the `tdd-guide` agent to automatically commit after each successful test cycle (green phase), with meaningful commit messages that reference the plan step being implemented.

---

## Background

Currently, the `tdd-guide` agent implements features using TDD but leaves all changes uncommitted. This assignment adds automatic commits at logical checkpoints:

```
TDD Cycle with Auto-Commits
───────────────────────────

Step 1: Add product filtering
├── RED: Write failing test
├── GREEN: Make test pass
├── COMMIT: "feat: add product filtering - step 1"  ← NEW
└── REFACTOR: Clean up

Step 2: Add filter UI component
├── RED: Write failing test
├── GREEN: Make test pass
├── COMMIT: "feat: add filter UI component - step 2"  ← NEW
└── REFACTOR: Clean up

... and so on
```

---

## Requirements

Update the `tdd-guide` agent to:

- Automatically commit at appropriate points in the TDD cycle
- Use meaningful commit messages that reference the plan step
- Never commit with failing tests
- Only stage relevant files

---

## Congratulations!

You've completed all assignments. Your development lifecycle now includes:

- Security review (Assignment 1)
- Full Jira-to-Done automation (Assignment 2)
- Auto-documentation (Assignment 3)
- Atomic commits (Assignment 4)

```
┌─────────────────────────────────────────────────────────────┐
│              Your Complete Development System               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  /full-lifecycle                                            │
│  ├── create-ticket (Jira)                                   │
│  ├── expand-ticket (Jira)                                   │
│  ├── planner (Agent)                                        │
│  ├── tdd-guide (Agent + auto-commits)                       │
│  ├── code-reviewer (Agent)                                  │
│  ├── security-reviewer (Agent)                              │
│  ├── documentation-agent (Agent)                            │
│  └── implement-ticket (Jira → Done)                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

From idea to documented, tested, reviewed, secure, committed code — all automated.
