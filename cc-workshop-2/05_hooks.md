# Workshop 05: Event-Driven Automation with Hooks

**Duration: ~15 minutes**

## What You'll Learn

- What hooks are and when they fire
- Hook configuration in settings.json
- Available environment variables in hooks
- Create validation, logging, and notification hooks

---

> **Windows Users:** Hook scripts in this module use bash shell syntax. You'll need **Git Bash** or **WSL** (Windows Subsystem for Linux) to run these hooks. Alternatively, you can create equivalent PowerShell scripts (`.ps1`) or batch files (`.bat`).

---

## What Are Hooks?

Hooks are **shell commands** that run in response to Claude Code events. They enable:

- **Validation** — Block dangerous operations before they execute
- **Logging** — Track all tool usage for audit
- **Notifications** — Alert when important actions complete
- **Automation** — Trigger follow-up actions automatically

---

## Hook Events

| Event | When It Fires | Use Case |
|-------|---------------|----------|
| `PreToolUse` | Before a tool runs | Validate, block dangerous ops |
| `PostToolUse` | After a tool completes | Log, notify, trigger follow-up |
| `Notification` | Claude sends notification | External alerts |
| `Stop` | Session ends | Cleanup, summary |

---

## Hook Configuration

Hooks are configured in `.claude/settings.json` (project) or `~/.claude/settings.json` (global):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "/path/to/pre-hook.sh"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "/path/to/post-hook.sh"
      }
    ]
  }
}
```

### Matcher Patterns

| Pattern | Matches |
|---------|---------|
| `"Bash"` | Any Bash tool use |
| `"Bash(git commit:*)"` | Git commit commands |
| `"Write"` | Any file write |
| `"*"` | All tool uses |

---

## Hook Environment Variables

Your hook scripts have access to these variables:

| Variable | Description | Available In |
|----------|-------------|--------------|
| `$TOOL_NAME` | The tool being used | Pre & Post |
| `$TOOL_INPUT` | Tool parameters (JSON) | Pre & Post |
| `$TOOL_OUTPUT` | Tool result | Post only |
| `$SESSION_ID` | Current session ID | Pre & Post |

---

## Hook Exit Codes

| Exit Code | Effect |
|-----------|--------|
| `0` | Continue (allow the tool) |
| Non-zero | Block (prevent the tool) |

This only applies to `PreToolUse` hooks.

---

## Task 1: Create a Pre-Commit Validation Hook

Block commits that contain "WIP" or "TODO" in the message.

### Step 1: Create hooks directory

```bash
# macOS/Linux
mkdir -p .claude/hooks

# Windows (Command Prompt)
mkdir .claude\hooks
```

### Step 2: Create the hook script

Create `.claude/hooks/pre-commit-check.sh`:

```bash
#!/bin/bash
# Block commits with WIP or TODO in message

# Parse the commit message from TOOL_INPUT (JSON)
MESSAGE=$(echo "$TOOL_INPUT" | grep -o '"message":"[^"]*"' | cut -d'"' -f4)

if echo "$MESSAGE" | grep -qiE "(WIP|TODO)"; then
    echo "BLOCKED: Commit message contains WIP or TODO"
    echo "Message was: $MESSAGE"
    exit 1
fi

exit 0
```

### Step 3: Make it executable (macOS/Linux only)

```bash
chmod +x .claude/hooks/pre-commit-check.sh
```

> **Windows:** This step is not needed. If using Git Bash, the script will run directly.

### Step 4: Configure in settings.json

Create or update `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit:*)",
        "command": ".claude/hooks/pre-commit-check.sh"
      }
    ]
  }
}
```

---

## Task 2: Create a Logging Hook

Log all tool usage for audit purposes.

### Create the hook script

Create `.claude/hooks/log-activity.sh`:

```bash
#!/bin/bash
# Log all tool usage to a file

LOG_FILE=".claude/logs/activity.log"
mkdir -p "$(dirname "$LOG_FILE")"

TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

echo "[$TIMESTAMP] Tool: $TOOL_NAME" >> "$LOG_FILE"
echo "  Session: $SESSION_ID" >> "$LOG_FILE"
echo "  Input: $TOOL_INPUT" >> "$LOG_FILE"
echo "" >> "$LOG_FILE"

exit 0
```

Make executable (macOS/Linux only):

```bash
chmod +x .claude/hooks/log-activity.sh
```

### Add to settings.json

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit:*)",
        "command": ".claude/hooks/pre-commit-check.sh"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "*",
        "command": ".claude/hooks/log-activity.sh"
      }
    ]
  }
}
```

---

## Task 3: Create a Notification Hook

Send a notification when a deployment command completes.

### Create the hook script

Create `.claude/hooks/notify-complete.sh`:

```bash
#!/bin/bash
# Notify on completion (customize for your notification system)

# Example: macOS notification
if command -v osascript &> /dev/null; then
    osascript -e "display notification \"Tool completed: $TOOL_NAME\" with title \"Claude Code\""
fi

# Example: Write to a notifications file
echo "[$(date '+%H:%M:%S')] Completed: $TOOL_NAME" >> .claude/logs/notifications.log

exit 0
```

Make executable (macOS/Linux only):

```bash
chmod +x .claude/hooks/notify-complete.sh
```

### Add selective notification

Only notify on git push:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "*",
        "command": ".claude/hooks/log-activity.sh"
      },
      {
        "matcher": "Bash(git push:*)",
        "command": ".claude/hooks/notify-complete.sh"
      }
    ]
  }
}
```

---

## Complete settings.json

Here's the full configuration:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit:*)",
        "command": ".claude/hooks/pre-commit-check.sh"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "*",
        "command": ".claude/hooks/log-activity.sh"
      },
      {
        "matcher": "Bash(git push:*)",
        "command": ".claude/hooks/notify-complete.sh"
      }
    ]
  }
}
```

---

## Diagram: Hook Execution Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude decides to use tool                │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
                ┌───────────────────┐
                │   PreToolUse      │
                │   Hooks Run       │
                │                   │
                │ • pre-commit-check│
                └─────────┬─────────┘
                          │
              ┌───────────┴───────────┐
              │                       │
        exit 0 (allow)          exit 1 (block)
              │                       │
              ▼                       ▼
      ┌───────────────┐       ┌───────────────┐
      │  Tool Runs    │       │  Tool Blocked │
      │               │       │  Error shown  │
      └───────┬───────┘       └───────────────┘
              │
              ▼
    ┌───────────────────┐
    │   PostToolUse     │
    │   Hooks Run       │
    │                   │
    │ • log-activity    │
    │ • notify-complete │
    └───────────────────┘
```

---

## Hook Best Practices

1. **Keep hooks fast** — They block execution while running
2. **Use exit codes** — 0 = continue, non-zero = block (PreToolUse only)
3. **Log hook errors** — Redirect stderr for debugging
4. **Test in isolation** — Run scripts manually first
5. **Use specific matchers** — Avoid `"*"` for PreToolUse hooks

---

## Testing Your Hooks

### Test the pre-commit block:

```
Commit these changes with message "WIP: work in progress"
```

Should be blocked by the pre-commit hook.

### Test logging:

After any tool use:

```bash
# macOS/Linux
cat .claude/logs/activity.log

# Windows
type .claude\logs\activity.log
```

Should show logged tool usage.

---

## Checkpoint

You've now:

- [ ] Understand hook events and configuration
- [ ] Created `.claude/hooks/pre-commit-check.sh` (validation)
- [ ] Created `.claude/hooks/log-activity.sh` (logging)
- [ ] Created `.claude/hooks/notify-complete.sh` (notification)
- [ ] Configured hooks in `.claude/settings.json`
- [ ] Tested hook execution

---

## Next Up

Continue to [06_full_workflow.md](./06_full_workflow.md) to see everything working together.
