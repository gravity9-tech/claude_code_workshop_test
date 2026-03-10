# Workshop 05: Parallel Workflows & Scaling

**Duration: ~15 minutes**

## What You'll Learn

- Git worktrees for isolated parallel Claude sessions
- Fan-out pattern: running Claude across multiple files/tasks
- Writer/reviewer pattern for quality at speed
- Managing multiple Claude sessions effectively

---

## The Problem

One Claude session works on one thing at a time. When you have multiple independent tasks — feature work, tests, reviews — you're bottlenecked.

```
Sequential (slow):
  Task A ──────────► Task B ──────────► Task C ──────────►
  |_____ 10 min _____|_____ 10 min _____|_____ 10 min _____|
  Total: 30 min

Parallel (fast):
  Task A ──────────►
  Task B ──────────►
  Task C ──────────►
  |_____ 10 min _____|
  Total: 10 min
```

---

## Git Worktrees

A **worktree** is an isolated copy of your repository with its own working directory and branch. Each Claude session gets its own worktree, so they can't step on each other's files.

### Creating a Worktree

```bash
# Start Claude in a new worktree
claude --worktree feature-auth

# Short form
claude -w bugfix-123
```

This creates:
- A new directory at `.claude/worktrees/feature-auth/`
- A new branch `worktree-feature-auth` based on the current branch
- A Claude session working in that isolated directory

### What Happens to Worktrees

| Scenario | Behavior |
|----------|----------|
| No changes made | Worktree and branch auto-deleted |
| Changes made | Claude prompts: keep or remove? |
| Keep | Directory and branch preserved for later |
| Remove | Directory deleted, changes discarded |

### Manual Worktree Management

You can also use git directly:

```bash
# Create worktree
git worktree add ../project-feature -b feature-branch

# Run Claude in it
cd ../project-feature && claude

# List all worktrees
git worktree list

# Clean up
git worktree remove ../project-feature
```

---

## Task 1: Parallel Feature + Tests

Run two Claude sessions simultaneously — one adds a feature, the other writes tests for it.

### Terminal 1: Feature Development

```bash
claude -w add-favorites
```

Then prompt:

```
Add a "favorites" feature to the Tea Store. Users should be able to
click a heart icon on any tea to add it to their favorites.
Add the heart icon to each product card and toggle state on click.
```

### Terminal 2: Test Writing

```bash
claude -w write-tests
```

Then prompt:

```
Look at the Tea Store product components. Write unit tests for
the product card component, covering rendering, user interactions,
and edge cases. Put tests alongside the components.
```

Both sessions work independently in isolated worktrees. Neither can interfere with the other.

### Merge Results

When both finish:

```bash
# Check what branches were created
git branch | grep worktree

# Merge the feature branch
git merge worktree-add-favorites

# Merge the test branch
git merge worktree-write-tests
```

---

## Fan-Out Pattern

Use `claude -p` in a loop to process multiple files or tasks in parallel.

### Lint Fix Across Files

```bash
# Find files with lint issues and fix them in parallel
eslint src/ --format json 2>/dev/null | \
  jq -r '.[] | select(.errorCount > 0) | .filePath' | \
  xargs -P 4 -I {} bash -c '
    cat "{}" | claude -p "Fix the lint issues in this file. Output only the corrected code." \
      --model haiku --max-turns 1 > "{}.fixed" && mv "{}.fixed" "{}"
  '
```

### Generate Docs for Multiple Files

```bash
# Generate JSDoc for all exported functions
find src/ -name "*.ts" | head -10 | \
  xargs -P 4 -I {} bash -c '
    claude -p "Add JSDoc comments to all exported functions in this file. Only output the file." \
      --model sonnet --max-turns 1 < "{}" > "{}.tmp" && mv "{}.tmp" "{}"
  '
```

### Process Multiple Tickets

```bash
# Process a list of tickets
echo "TEA-101 TEA-102 TEA-103" | tr ' ' '\n' | \
  xargs -P 3 -I {} claude -p "Analyze Jira ticket {} and suggest an implementation approach" \
    --model sonnet --max-turns 3 --max-budget-usd 0.50
```

**Key:** `-P 4` tells `xargs` to run 4 processes in parallel. Adjust based on your API rate limits.

---

## Writer/Reviewer Pattern

One Claude session writes code, another reviews it. This catches issues that a single session might miss.

### Step 1: Writer Session

```bash
claude -p "Implement a rate limiter middleware for the Express API. \
  Add it to src/middleware/rate-limiter.ts. \
  Use a sliding window algorithm with configurable limits." \
  --model sonnet --max-turns 10
```

### Step 2: Reviewer Session

```bash
git diff HEAD~1 | claude -p "Review this code change. \
  Check for: bugs, security issues, edge cases, performance problems. \
  Be specific and cite line numbers." \
  --model sonnet --max-turns 3
```

### Automated Writer/Reviewer Script

```bash
#!/bin/bash
# write-and-review.sh

TASK="$1"

echo "=== WRITING ==="
claude -p "$TASK" --model sonnet --max-turns 10

echo ""
echo "=== REVIEWING ==="
git diff | claude -p "Review this code change for bugs, security, and quality. Be concise." \
  --model sonnet --max-turns 3
```

Usage:

```bash
./write-and-review.sh "Add input validation to the checkout form"
```

---

## Agent-Level Isolation

Custom agents (from Module 03) can also run in worktrees:

```yaml
---
name: feature-builder
description: Implements features in an isolated worktree
tools: Read, Grep, Glob, Bash, Edit, Write
isolation: worktree
---

Implement the requested feature in this isolated worktree.
When done, commit your changes and report what was done.
```

The `isolation: worktree` setting ensures this agent always gets its own copy of the repo.

---

## Multi-Session Tips

### Name Your Sessions

```bash
# In session 1
/rename feature-auth

# In session 2
/rename tests-auth

# Resume later
claude -r feature-auth
claude -r tests-auth
```

### Desktop/Web Multi-Session

On Claude Desktop or claude.ai, you can have multiple conversation tabs. Each is an independent session — useful for the writer/reviewer pattern without needing terminal multiplexing.

### Cost Awareness

Parallel sessions multiply costs. A single session costing $0.50 becomes $2.00 when running 4 in parallel. Always use `--max-budget-usd` in automated scenarios.

---

## Key Takeaways

- **Worktrees** give each Claude session an isolated repo copy — no file conflicts
- **`claude -w <name>`** creates a worktree session in one command
- **Fan-out** with `xargs -P` to process multiple files/tasks in parallel
- **Writer/reviewer** catches issues a single session would miss
- **Always set budget limits** when running parallel sessions
- Agents with `isolation: worktree` get automatic isolation

---

Continue to: [05_full_workflow.md](./05_full_workflow.md)
