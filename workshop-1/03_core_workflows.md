# Workshop 03: Core Workflows

**Duration: ~15 minutes**

## What You'll Learn

- Claude's built-in tools and how it uses them
- Interactive mode vs print mode
- Plan mode for safe exploration
- Session management: continue, resume, rename
- Checkpoints and `/rewind`

---

## Two Ways to Use Claude Code

### Interactive Mode (default)

Start a conversation, go back and forth:

```bash
claude
```

This is your primary workflow — ask questions, make changes, iterate.

### Print Mode (`-p`)

Single query, get a response, exit:

```bash
claude -p "What does this project do?"
```

Useful for scripting, CI/CD, and quick one-off questions. You'll use this more in Workshop 2.

---

## Claude's Built-in Tools

When Claude works, it uses specific tools under the hood. You'll see these in the output as Claude acts:

| Tool | What it does | Example |
|------|-------------|---------|
| **Read** | Read file contents | Reading `src/index.ts` |
| **Glob** | Find files by pattern | Searching for `**/*.test.ts` |
| **Grep** | Search file contents | Finding `TODO` across the codebase |
| **Edit** | Modify part of a file | Replacing a function body |
| **Write** | Create or overwrite a file | Creating `config.json` |
| **Bash** | Run shell commands | `npm test`, `git status` |
| **Task** | Spawn a subagent | Delegating research to an Explore agent |

Claude picks the right tool automatically. When you say "find all files that import axios," Claude uses **Grep**. When you say "run the tests," it uses **Bash**.

### Task 1: Watch the Tools in Action

Ask Claude to do something and watch which tools it uses:

```
Find all TypeScript files in this project that contain the word "error",
then read the first one and explain what it does
```

You should see Claude use **Glob** (find files), **Grep** (search contents), and **Read** (read the file) in sequence.

---

## Plan Mode

Plan mode lets Claude **read and explore** without making any changes. Perfect for understanding code before modifying it.

### Enter Plan Mode

Ask claude to enter plan mode to work.

modes:
- **Normal** → Claude can read and write
- **Plan** → Claude can only read (no edits, no commands that modify)

### The Explore → Plan → Implement Pattern

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Explore    │ ──► │     Plan     │ ──► │  Implement   │
│  (Plan Mode) │     │  (Plan Mode) │     │(Normal Mode) │
│              │     │              │     │              │
│ "Read src/   │     │ "Create a    │     │ "Implement   │
│  auth/ and   │     │  plan for    │     │  the plan"   │
│  explain"    │     │  adding      │     │              │
│              │     │  OAuth"      │     │              │
└──────────────┘     └──────────────┘     └──────────────┘
```

This prevents wasting tokens on wrong implementations.

---

## Task 2: Try Plan Mode

Switch to plan mode:

```
Using plan mode, read the main entry point of this project and explain the architecture
```

---

## Task 3: Explore Then Implement

Stay in plan mode:

```
Using plan mode, what would need to change to add a health check endpoint?
```

Review Claude's plan. If it looks good, switch to normal mode:

```
Implement the health check endpoint from your plan
```

---

## Session Management

Sessions persist locally. You can leave and come back.

### Continue the last session

```bash
claude -c
```

Picks up exactly where you left off in the current directory.

### Resume a specific session

```bash
claude -r
```

Shows a picker of recent sessions. Choose one to resume.

### Name your sessions

Inside a session, run:

```bash
/rename auth-refactor
```

Now you can resume by name:

```bash
claude -r auth-refactor
```

---

## Task 4: Practice Session Management

1. Start a new Claude Code session
2. Ask Claude something about your project
3. Run `/rename workshop-practice`
4. Exit the session (`Ctrl+C` or type `/exit`)
5. Resume it: `claude -r workshop-practice`
6. Verify Claude remembers the previous conversation

---

## Checkpoints & Rewind

Every action Claude takes creates a checkpoint. You can restore to any previous point.

### Open the rewind menu

```bash
/rewind
```

Or press `Esc` twice.

This shows a list of checkpoints. You can:
- **Restore conversation** — roll back messages
- **Restore code** — revert file changes
- **Restore both** — full rollback
- **Summarize from here** — compact from a specific point

---

## Task 5: Rewind a Change

1. Ask Claude to make a small code change (e.g., add a comment to a file)
2. Verify the change was made
3. Run `/rewind`
4. Select the checkpoint before the change
5. Choose "Restore both"
6. Verify the change was reverted

This is safer than "undo that" — it actually removes the attempt from context.

---

## Key Takeaways

- Use **plan mode** to explore before implementing
- **Name sessions** for easy resumption
- **`/rewind`** to cleanly undo mistakes (removes from context too)
- **`-c`** to continue, **`-r`** to resume by name

---

Continue to: [04_claude_md.md](./04_claude_md.md)
