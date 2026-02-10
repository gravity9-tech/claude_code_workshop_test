# Part 3: Create the Smart Commit Command

**Estimated Time:** 10 minutes

## Objective

Create a slash command that orchestrates the full commit workflow: check for staged changes, spawn the commit-writer agent, present the message, and execute the commit.

## Background

A **slash command** is an orchestration script that chains multiple operations together. Commands use the **Task tool** to spawn agents as subagents in isolated contexts.

The key difference from running an agent directly:
- Commands can include pre/post steps (checking for changes, executing the commit)
- Commands can handle errors and branching logic
- Commands provide a simple user interface (`/smart-commit`)

## Your Task

Use the `/command-creator` to create a command called `smart-commit`.

### The Workflow

```
/smart-commit
      │
      ▼
┌─────────────────┐
│ 1. CHECK        │  Run: git diff --staged --quiet
│    Are there    │  If exit code 0 → no changes, stop
│    staged       │  If exit code 1 → changes exist, continue
│    changes?     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 2. GENERATE     │  Spawn commit-writer agent (via Task tool)
│    Commit       │  Agent runs in isolated context
│    message      │  Returns: suggested commit message
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 3. PRESENT      │  Show the suggested message to user
│    Show message │  Ask: "Use this message? (yes/edit/cancel)"
│    to user      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 4. COMMIT       │  If confirmed: git commit -m "<message>"
│    Execute      │  Show the commit result
└─────────────────┘
```

---

## Instructions

In Claude Code, type:

```
/command-creator Create a slash command called "smart-commit"

The command orchestrates a git commit workflow:

1. CHECK PHASE — Verify staged changes exist:
   - Run: git diff --staged --quiet
   - If no changes are staged, inform the user and stop
   - If changes exist, continue to the next phase

2. GENERATE PHASE — Spawn the commit-writer agent:
   - Use the Task tool to launch the "commit-writer" agent
   - The agent analyzes staged changes and generates a commit message
   - Capture the suggested commit message from the agent's response

3. PRESENT PHASE — Show the message to the user:
   - Display the suggested commit message clearly
   - Ask the user to confirm: use as-is, edit, or cancel
   - If the user wants to edit, let them provide modifications

4. COMMIT PHASE — Execute the commit:
   - If confirmed, run: git commit -m "<final message>"
   - Show the commit result (hash, files changed)
   - If cancelled, inform the user no commit was made

The command should:
- Use the Task tool to spawn the commit-writer agent
- Handle the case where there are no staged changes gracefully
- Allow the user to modify the suggested message before committing
- Never commit without user confirmation

Tools needed: Task (for spawning agent), Bash (for git commands)
```

---

## Verify Your Work

Check that the command was created by opening it in your text editor:

```
.claude/commands/smart-commit.md
```

Verify:
- [ ] `allowed-tools:` includes `Task` and `Bash`
- [ ] Workflow checks for staged changes first
- [ ] Uses Task tool to spawn the commit-writer agent
- [ ] Asks for user confirmation before committing

---

## Test the Command

Restart Claude Code to load the new command:

```bash
claude
```

Verify the command is available:

```
/help
```

You should see `/smart-commit` in the list.

---

## Next

Continue to [04_verification.md](./04_verification.md) to test the complete workflow.
