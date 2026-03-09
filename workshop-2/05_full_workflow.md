# Workshop 05: Full Workflow Exercise

**Duration: ~15 minutes**

## What You'll Do

Put everything from this workshop together. You'll take a feature request and automate the entire flow — from permissions to PR — using hooks, agents, and parallel sessions.

---

## The Scenario

A new feature request comes in: **"Add a search bar to the Tea Store."**

You'll handle it using the tools from every module:

```
Permissions ──► Hooks ──► Agents ──► Parallel Work ──► PR
   (M01)        (M02)     (M03)        (M04)
     │            │         │             │
     ▼            ▼         ▼             ▼
  Scoped       Auto-lint  Test &       Feature +
  access       on edits   review       tests in
                                       parallel
```

---

## Step 1: Verify Your Setup

Confirm your project has the pieces from earlier modules:

```bash
# Check permissions and hooks are configured
cat .claude/settings.json
```

You should see permission rules (Module 01) and hooks (Module 02).

```bash
# Check agents exist
ls .claude/agents/
```

You should see `test-runner.md` and `reviewer.md` (Module 03).

If anything is missing, go back to the relevant module and create it.

---

## Step 2: Implement the Feature

Start a clean session:

```bash
claude
```

```
/clear
```

Ask Claude to implement the search feature:

```
Add a search bar to the Tea Store. It should:
- Appear at the top of the product list
- Filter products in real-time as the user types
- Search by product name and description
- Show "No results found" when nothing matches
- Clear button to reset the search
```

Watch what happens:
- **Permissions** from Module 01 allow Claude to read/edit source files and run tests
- **Hooks** from Module 02 auto-lint any JS/TS files Claude edits
- Claude uses its tools (Read, Edit, Bash) within the scoped rules

---

## Step 3: Run the Agents

After Claude implements the feature, use your custom agents:

### Run Tests (deterministic)

```
@test-runner run all test suites and report results
```

The `@test-runner` invocation guarantees the test-runner agent handles it. It runs in its own context, executes Vitest + Pytest, and reports back with a structured summary.

### Review the Code (probabilistic)

```
check the search feature implementation for quality issues
```

Claude should recognize this as a review task and delegate to the reviewer agent based on its description. If it doesn't, you can always force it:

```
@reviewer check the search feature implementation
```

Fix any issues the agents surface, then move on.

---

## Step 4: Commit and Create the PR

```
Create a branch called feature/search-bar, commit the changes,
and open a PR with a description explaining the search bar feature.
```

---

## Bonus: Parallel Approach

Try the worktree approach from Module 04:

### Terminal 1: Implement the feature

```bash
claude -w search-feature
```

```
Add a search bar to the Tea Store that filters products by name and description
```

### Terminal 2: Write tests for it

```bash
claude -w search-tests
```

```
Write tests for a Tea Store search bar component that filters products
by name and description. Include tests for: rendering, typing to filter,
clearing search, and no results state.
```

Merge both when done:

```bash
git merge worktree-search-feature
git merge worktree-search-tests
```

---

## Workshop Recap

In this workshop you learned:

| Module | What You Learned |
|--------|-----------------|
| 01 | Permission modes and allow/deny rules control what Claude can do |
| 02 | Hooks automate responses to Claude events (guard + react) |
| 03 | Custom agents with deterministic (`@name`) and probabilistic invocation |
| 04 | Worktrees and fan-out patterns scale Claude to parallel tasks |
| 05 | Everything together: permissions → hooks → agents → parallel → PR |

## What You Built

```
your-project/
├── .claude/
│   ├── agents/
│   │   ├── test-runner.md        # Runs tests, reports results
│   │   └── reviewer.md           # Reviews code against conventions
│   ├── settings.json             # Permissions + hooks
│   └── skills/                   # From Workshop 1
├── CLAUDE.md                     # Project instructions
└── ...
```

## What's Next

You now have the building blocks for automated development workflows. Combine these patterns for your team:

- **Test-on-commit**: Hooks trigger test agents after every commit
- **Parallel sprints**: Worktrees for multiple features simultaneously
- **Agent-driven reviews**: `@reviewer` after every implementation
- **Ticket-to-PR**: Skills (Workshop 1) + agents = automated pipeline

The more you codify into permissions, hooks, and agents, the more consistent and automated your workflow becomes.
