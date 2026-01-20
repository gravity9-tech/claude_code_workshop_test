# Workshop 06: Hooks

**Duration: ~6 minutes**

## What You'll Learn

- What hooks are and when they trigger
- Configure hooks in settings
- Create file protection hooks
- Understand hook events

---

## What Are Hooks?

**Hooks** are shell commands that run automatically when Claude Code events occur. Use them to:

- Protect sensitive files from edits
- Log tool usage
- Trigger notifications
- Run validation before/after changes

---

## Hook Events

| Event | When It Fires |
|-------|---------------|
| `PreToolUse` | Before a tool runs |
| `PostToolUse` | After a tool completes |
| `Notification` | When Claude sends a notification |
| `Stop` | When Claude stops working |

---

## Configuration Location

Hooks live in settings files:

- **Project:** `.claude/settings.json`
- **User:** `~/.claude/settings.json`

**Important:** You must restart Claude Code after changing hooks.

---

## Hook Format

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Running bash command'"
          }
        ]
      }
    ]
  }
}
```

### Matcher Patterns

| Pattern | Matches |
|---------|---------|
| `"Bash"` | Bash tool only |
| `"Edit"` | Edit tool only |
| `"Edit\|Write"` | Edit OR Write tools |
| `""` | All tools (empty string) |

### Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success, continue |
| 2 | Block the operation |
| Other | Warning, continue |

---

## Task 1: Create Settings File

```bash
mkdir -p .claude
touch .claude/settings.json
```

---

## Task 2: Add File Protection Hook

The dev-tools plugin uses a hook to protect sensitive files. Let's create one:

Create `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$TOOL_INPUT\" | grep -qE '\\.(env|pem|key)$|secrets/|credentials'; then echo 'BLOCKED: Cannot edit sensitive files' >&2; exit 2; fi"
          }
        ]
      }
    ]
  }
}
```

This blocks edits to:
- `.env` files
- `.pem` and `.key` files
- Anything in `secrets/` directory
- Files with "credentials" in the path

---

## Task 3: Restart Claude Code

Hooks only load at startup:

```bash
# Exit Claude Code (Ctrl+C)
# Restart
claude
```

---

## Task 4: Test the Hook

Try to edit a protected file:

```
Create a file called test.env with some content
```

You should see the hook block the operation:

```
BLOCKED: Cannot edit sensitive files
```

---

## Task 5: Add Logging Hook

Add a hook to log all tool usage:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "if echo \"$TOOL_INPUT\" | grep -qE '\\.(env|pem|key)$|secrets/|credentials'; then echo 'BLOCKED: Cannot edit sensitive files' >&2; exit 2; fi"
          }
        ]
      },
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"[$(date +%H:%M:%S)] Tool: $TOOL_NAME\" >> .claude/tool.log"
          }
        ]
      }
    ]
  }
}
```

Now all tool usage is logged to `.claude/tool.log`.

---

## Task 6: Add Notification Sound (macOS)

Add a sound when Claude finishes:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Glass.aiff"
          }
        ]
      }
    ]
  }
}
```

*Note: `afplay` is macOS. Use `paplay` on Linux.*

---

## Common Hook Patterns

### Block Dangerous Commands

```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "if echo \"$TOOL_INPUT\" | grep -qE 'rm -rf|sudo|chmod 777'; then exit 2; fi"
  }]
}
```

### Auto-Format After Edit

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "npx prettier --write \"$TOOL_INPUT\" 2>/dev/null || true"
  }]
}
```

### Notify on Completion

```json
{
  "matcher": "",
  "hooks": [{
    "type": "command",
    "command": "osascript -e 'display notification \"Claude finished\" with title \"Claude Code\"'"
  }]
}
```

---

## Your Structure

```
.claude/
├── commands/
│   ├── test-feature.md
│   └── quick-check.md
├── agents/
│   └── feature-builder.md
└── settings.json              # Hooks config
```

---

## Checkpoint

Before continuing, verify:

- [ ] Created `.claude/settings.json`
- [ ] Added file protection hook
- [ ] Restarted Claude Code
- [ ] Tested that protected files are blocked
- [ ] Understand hook events and matchers

---

## What You've Learned

1. **Hooks** run shell commands on Claude Code events
2. **PreToolUse** runs before tools, can block with exit 2
3. **Matcher patterns** filter which tools trigger hooks
4. **Must restart** Claude Code after changing hooks

---

## Next Up

Time to see it all come together!

Continue to: [07_full_workflow.md](./07_full_workflow.md) - End-to-end orchestration
