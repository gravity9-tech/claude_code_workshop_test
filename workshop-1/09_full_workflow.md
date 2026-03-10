# Workshop 09: Full Workflow Exercise

**Duration: ~10 minutes**

## What You'll Do

Put everything from this workshop together in one end-to-end exercise.

---

## The Challenge

Pick a real (or test) Jira ticket and take it from open to PR — using only Claude Code.

```
Jira Ticket ──► Explore ──► Implement ──► Test ──► Commit ──► PR
     │                                                         │
     │              Everything through Claude Code             │
     └─────────────────────────────────────────────────────────┘
```

---

## Step 1: Prepare

Start a **clean session**:

```bash
/clear
```

Verify your setup:

```bash
/mcp
```

Confirm `atlassian` is connected. Check your CLAUDE.md is loaded:

```bash
/memory
```

---

## Step 2: Run `/fix-issue`

Pick a ticket from your Jira project and run:

```
/fix-issue PROJECT-123
```

Watch Claude work through the full workflow:

1. Reads the Jira ticket via MCP
2. Explores relevant code
3. Plans the approach
4. Implements the fix
5. Runs tests
6. Commits with ticket reference

---

## Step 3: Review and Open a PR

Review what Claude did:

```
Show me the git diff for the changes you just made
```

If the changes look good:

```
Create a branch called fix/PROJECT-123, push it, and open a PR.
Include the Jira ticket link in the PR description.
```

If something needs changing:

```
The error handling needs to cover the null case too. Fix that,
then create the branch and PR.
```

---

## Step 4: Verify

Check the results:

- Open the PR URL Claude gives you
- Review the diff in GitHub
- Check that the commit message references the ticket
- Verify tests are passing

---

## Bonus: Token Check

Run `/stats` to see how many tokens the full workflow consumed. Compare this to what you'd expect from doing each step manually.

---

## Workshop Recap

In this workshop you learned:

| Module | What You Learned |
|--------|-----------------|
| 01 | Context window is your most important resource |
| 02 | `/clear`, `/compact`, `/rewind` to manage tokens |
| 03 | Plan mode, sessions, checkpoints |
| 04 | CLAUDE.md gives Claude persistent project knowledge |
| 05 | Specific prompts + verification = better results |
| 06 | Git and PR workflows with `gh` |
| 07 | MCP servers connect Claude to external tools |
| 08 | Skills automate repeatable workflows |
| 09 | Everything together: ticket → code → PR |

## What's Next

**Workshop 2** covers:
- Custom agents (isolated workers with focused tasks)
- Hooks (automated actions on Claude events)
- CI/CD integration with `claude -p`
- Parallel workflows and scaling
- Permissions and security

---

## Your Project After This Workshop

```
your-project/
├── .claude/
│   └── skills/
│       ├── commit-style/
│       │   └── SKILL.md       # Commit message standards
│       └── fix-issue/
│           └── SKILL.md       # Jira ticket → fix → commit
├── .mcp.json                  # Atlassian MCP (if project-scoped)
└── CLAUDE.md                  # Project instructions
```
