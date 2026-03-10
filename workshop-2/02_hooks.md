# Workshop 02: Hooks

**Duration: ~20 minutes**

## What You'll Learn

- What hooks are and when they fire
- The four hook types: command, HTTP, prompt, and agent
- Matcher patterns for targeting specific tools
- How to configure hooks in settings files

---

## What Are Hooks?

Hooks are **deterministic automations** that run in response to Claude Code events. Unlike agents (which make decisions), hooks execute the same way every time.

```
┌──────────────┐     Event fires     ┌──────────────┐
│  Claude Code │ ──────────────────► │    Hook      │
│  does thing  │                     │  (your code) │
└──────────────┘                     └──────┬───────┘
                                            │
                                     ┌──────┴───────┐
                                     │   Result:    │
                                     │  allow/block │
                                     │  + message   │
                                     └──────────────┘
```

**Use cases:**
- Block dangerous commands before they run
- Auto-lint files after edits
- Log all tool usage for auditing
- Validate prompts before processing
- Send notifications when Claude finishes

---

## Hook Events

Hooks fire at specific points in Claude Code's lifecycle:

| Event | When it fires | Can block? |
|-------|--------------|------------|
| `PreToolUse` | Before a tool call executes | Yes |
| `PostToolUse` | After a tool call succeeds | Yes (retry) |
| `PostToolUseFailure` | After a tool call fails | No |
| `UserPromptSubmit` | When you submit a prompt | Yes |
| `Stop` | When Claude finishes responding | Yes (continue) |
| `SessionStart` | When a session begins/resumes | No |
| `SubagentStart` | When a subagent is spawned | No |
| `SubagentStop` | When a subagent finishes | No |
| `Notification` | When Claude sends a notification | No |

The most useful for automation are **`PreToolUse`** (guard against bad actions) and **`PostToolUse`** (trigger follow-up actions).

---

Structure breakdown:

```
hooks
  └── EVENT_NAME (e.g., PreToolUse)
        └── array of rule objects
              ├── matcher — regex pattern for which tools to match
              └── hooks — array of hook actions to run
                    ├── type — command, http, prompt, or agent
                    └── type-specific config (command, url, prompt, etc.)
```

---

## Matcher Patterns

The `matcher` field is a **regex** that filters which tools trigger the hook:

| Matcher | Matches |
|---------|---------|
| `"Bash"` | Only Bash tool calls |
| `"Edit\|Write"` | Edit or Write tool calls |
| `"mcp__.*"` | Any MCP tool call |
| `".*"` | Everything |
| (omitted) | Everything (same as `.*`) |

For `PreToolUse` and `PostToolUse`, the matcher applies to the **tool name**. For other events, see the docs for event-specific matchers.

---

## Task 1: Block Dangerous Commands

Create a `PreToolUse` hook that prevents `rm -rf` commands. Add this to your `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "INPUT=$(cat) && COMMAND=$(echo \"$INPUT\" | jq -r '.tool_input.command // empty') && if echo \"$COMMAND\" | grep -qE 'rm\\s+-(r|rf|fr)\\b'; then echo 'Blocked: rm -rf/rm -r commands are not allowed' >&2 && exit 2; else exit 0; fi"
          }
        ]
      }
    ]
  }
}
```

How this works:
1. Claude attempts a Bash tool call
2. The `PreToolUse` event fires with the tool input as JSON on stdin
3. The hook reads the command from `.tool_input.command`
4. If it contains `rm -rf`, exit code `2` blocks it
5. Otherwise, exit code `0` allows it

### Verify It Works

Start a new Claude Code session and try:

```
Run rm -rf node_modules to clean up
```

Claude should report that the command was blocked by the hook.

Now try a safe command:

```
Run ls -la
```

This should work normally.

---

## Key Takeaways

- **Hooks are deterministic** — they run the same way every time (unlike agents)
- **`PreToolUse`** to guard, **`PostToolUse`** to react
- **Command hooks** for scripts, **prompt hooks** for intelligent evaluation
- **Matcher patterns** are regex — target specific tools or match everything
- **Exit code 2** blocks the action; **exit code 0** allows it
- Combine hooks with permissions for layered security

---

Continue to: [03_custom_agents.md](./03_custom_agents.md)
