# Workshop 01: Permissions & Security

**Duration: ~15 minutes**

## What You'll Learn

- Claude Code's permission modes and when to use each
- How to write allow/deny rules for specific tools and commands
- The settings hierarchy and how rules cascade
- Filesystem and network sandboxing

---

## Why This Matters

Everything in this workshop — hooks, agents, CI/CD — involves Claude taking actions automatically. Before you automate anything, you need to control **what Claude is allowed to do**.

```
┌──────────────────────────────────────────────────┐
│                Permission Layer                   │
├──────────────────────────────────────────────────┤
│                                                  │
│   Hooks ──► run automatically                    │
│   Agents ──► work independently                  │
│   CI/CD ──► no human in the loop                 │
│                                                  │
│   All of these need guardrails.                  │
│                                                  │
└──────────────────────────────────────────────────┘
```

---

## Permission Modes

Claude Code has five permission modes that control how tool use is approved:

| Mode | Behavior | Use for |
|------|----------|---------|
| `default` | Prompts you before first use of each tool | Day-to-day work |
| `acceptEdits` | Auto-approves file edits, prompts for everything else | Trusted coding sessions |
| `plan` | Read-only — no edits, no commands that modify | Safe exploration |
| `dontAsk` | Auto-denies everything unless pre-approved in rules | Locked-down automation |
| `bypassPermissions` | Skips all checks | Isolated CI environments only |

### How to Set the Mode

**Per-session** (CLI flag):

```bash
claude --permission-mode plan
```

**In settings** (persistent):

```json
{
  "permissions": {
    "defaultMode": "default"
  }
}
```

### Task 1: Try Plan Mode

Start Claude Code in plan mode:

```bash
claude --permission-mode plan
```

Try asking Claude to edit a file:

```
Add a comment to the top of package.json
```

Claude will explore and plan but won't make changes. This is how you safely investigate code before committing to an approach.

Exit and restart normally.

---

## Allow/Deny Rules

Rules let you pre-approve or block specific tools and commands without being prompted each time.

### Basic Syntax

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep"
    ],
    "deny": [
      "Write"
    ]
  }
}
```

### Command-Specific Rules (Bash)

The real power is in pattern matching for shell commands:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(npm run build)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push *)",
      "Bash(git reset --hard *)"
    ]
  }
}
```

`*` is a wildcard — `Bash(git diff *)` matches `git diff main`, `git diff --staged`, etc.

### File Path Rules (Edit/Read)

Restrict which files Claude can read or modify:

```json
{
  "permissions": {
    "allow": [
      "Edit(src/**)",
      "Read(src/**)"
    ],
    "deny": [
      "Read(.env)",
      "Edit(package-lock.json)"
    ]
  }
}
```

### MCP Tool Rules

Control access to MCP server tools:

```json
{
  "permissions": {
    "allow": [
      "mcp__github__*"
    ],
    "deny": [
      "mcp__slack__send_message"
    ]
  }
}
```

### Rule Priority

Within any settings file, rules are evaluated in this order:

1. **`deny`** — highest priority, blocks matching actions
2. **`ask`** — prompts for approval
3. **`allow`** — lowest priority, auto-approves

If a command matches both `allow` and `deny`, **deny wins**.

---

## Settings Hierarchy

Rules can live in multiple files. They cascade from highest to lowest priority:

```
┌─────────────────────────────┐
│  1. Managed policy settings │  ← Admin-controlled, can't override
├─────────────────────────────┤
│  2. CLI flags               │  ← --allowedTools, --disallowedTools
├─────────────────────────────┤
│  3. Local project settings  │  ← .claude/settings.local.json
├─────────────────────────────┤
│  4. Project settings        │  ← .claude/settings.json (shared)
├─────────────────────────────┤
│  5. User settings           │  ← ~/.claude/settings.json
└─────────────────────────────┘
```

| File | Path | Shared with team? |
|------|------|-------------------|
| User settings | `~/.claude/settings.json` | No |
| Project settings | `.claude/settings.json` | Yes (version control) |
| Local project settings | `.claude/settings.local.json` | No (gitignore it) |

**Project settings** (`.claude/settings.json`) are the most important for teams — they get checked into git so everyone has the same rules.

---

## Sandboxing

Claude Code runs with two layers of sandboxing beyond permissions:

### Filesystem Sandbox

Claude can only access files within your project directory and its subdirectories. It cannot read `/etc/passwd` or your SSH keys.

### Network Sandbox

Network access from Bash commands is restricted. Claude cannot make arbitrary outbound HTTP requests from the shell unless explicitly allowed.

These sandboxes apply even in `bypassPermissions` mode — they're a separate security layer.

---

## Task 2: Configure Project Permissions

Create a `.claude/settings.json` that allows safe development commands but blocks destructive operations.

Create the file:

```bash
mkdir -p .claude
```

Add this to `.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(npm run build)",
      "Bash(npx *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Bash(git branch *)",
      "Bash(ls *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)",
      "Bash(git reset --hard *)",
      "Read(.env)"
    ]
  }
}
```

### Task 3: Verify a Denied Command

Start a Claude Code session and try:

```
Run rm -rf node_modules
```

Claude should be blocked from executing this. You'll see a denial message instead of the command running.

Now try an allowed command:

```
Run npm test
```

This should execute without any permission prompt.

---

## Key Takeaways

- **`default` mode** for normal work, **`plan`** for safe exploration, **`dontAsk`** for locked-down automation
- **`deny` rules beat `allow` rules** — use deny for dangerous operations
- **Project settings** (`.claude/settings.json`) are shared via git — use them for team-wide rules
- **Sandboxing** restricts filesystem and network access as a separate safety layer
- Set up permissions **before** automating with hooks, agents, or CI/CD

---

Continue to: [02_hooks.md](./02_hooks.md)
