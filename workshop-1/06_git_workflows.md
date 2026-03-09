# Workshop 06: Git & PR Workflows

**Duration: ~15 minutes**

## What You'll Learn

- How Claude Code works with git natively
- Creating commits with descriptive messages
- Creating branches and opening pull requests
- Reviewing diffs before committing

---

## Claude Knows Git

Claude Code can run git commands directly through its Bash tool. Combined with the `gh` CLI, it can handle the full git workflow: staging, committing, branching, and opening PRs.

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Stage &   │ ──► │   Create    │ ──► │   Open PR   │
│   Commit    │     │   Branch    │     │   on GitHub  │
│             │     │             │     │              │
│  git add    │     │  git branch │     │  gh pr create│
│  git commit │     │  git push   │     │              │
└─────────────┘     └─────────────┘     └─────────────┘
```

No plugins or extensions needed — just Claude Code and `gh`.

---

## Prerequisites

Verify `gh` is installed and authenticated:

```bash
gh auth status
```

If not authenticated:

```bash
gh auth login
```

---

## Task 1: Make a Change and Commit

Ask Claude to make a small change and commit it:

```
Add a comment at the top of README.md with today's date. Then commit it with a descriptive message.
```

Watch what Claude does:
1. Reads the file
2. Makes the edit
3. Runs `git add` with specific files (not `git add .`)
4. Crafts a commit message
5. Runs `git commit`

---

## Task 2: Review a Diff Before Committing

Make a change but ask Claude to **show you the diff first**:

```
Add a .editorconfig file with 2-space indentation for JS/TS files.
Show me the diff before committing.
```

Review the diff. If it looks good:

```
Looks good, commit it
```

If not:

```
Change the indentation to 4 spaces instead
```

This is the review workflow — always inspect before committing.

---

## Task 3: Create a Branch and PR

Ask Claude to create a feature branch and open a PR:

```
Create a branch called workshop-test, add a CONTRIBUTING.md file with
basic contribution guidelines, commit it, push, and open a PR.
```

Claude will:
1. Create and switch to the branch
2. Write the file
3. Commit with a descriptive message
4. Push to origin with `-u`
5. Run `gh pr create` with a title and description

Check the PR URL Claude returns — open it in your browser to verify.

---

## Task 4: Clean Up

Close the test PR and delete the branch:

```
Close the PR you just created and delete the workshop-test branch
```

---

## Useful Git Prompts

| Task | Prompt |
|------|--------|
| Commit current work | "Commit my changes with a descriptive message" |
| Review before commit | "Show me the git diff, then commit if it looks correct" |
| Branch + PR | "Create a branch, commit, and open a PR" |
| Fix commit message | "Amend the last commit message to be more descriptive" |
| View recent history | "Show me the last 5 commits with their messages" |
| Stash and switch | "Stash my changes and switch to main" |

---

## Key Takeaways

- Claude uses `git` and `gh` natively — no extra setup
- **Always review diffs** before letting Claude commit
- Claude writes descriptive commit messages automatically
- Full workflow: branch → implement → commit → push → PR

---

Continue to: [07_mcp_servers.md](./07_mcp_servers.md)
