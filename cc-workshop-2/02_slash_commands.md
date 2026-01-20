# Workshop 02: Slash Commands

**Duration: ~8 minutes**

## What You'll Learn

- What slash commands are
- How to create custom commands
- Using arguments with `$ARGUMENTS`
- Build a test command using Playwright

---

## What Are Slash Commands?

Slash commands are **reusable prompts** stored as Markdown files. Instead of typing the same instructions repeatedly, you type `/command-name` and Claude executes the saved prompt.

**Key difference from skills:**
- **Skills** = Auto-invoked based on description matching
- **Commands** = Explicitly invoked via `/name`

---

## Command Locations

| Location | Scope |
|----------|-------|
| `.claude/commands/` | Project-specific |
| `~/.claude/commands/` | Personal (all projects) |

---

## Command File Format

```markdown
---
description: Brief description shown in /help
argument-hint: <optional-args>
allowed-tools: Tool1, Tool2
---

Your prompt here. Use $ARGUMENTS for user input.
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `description` | No | Shown in autocomplete |
| `argument-hint` | No | Shows expected arguments |
| `allowed-tools` | No | Restrict which tools can be used |
| `model` | No | Override the model |

---

## Task 1: Create Commands Directory

```bash
mkdir -p .claude/commands
```

---

## Task 2: Create a Test Command

Let's create a command that tests a feature using Playwright.

Create `.claude/commands/test-feature.md`:

```markdown
---
description: Test a feature on localhost using Playwright
argument-hint: <feature-to-test>
---

Test the following feature on http://localhost:4210 using Playwright MCP:

## Feature to Test

$ARGUMENTS

## Instructions

1. Navigate to http://localhost:4210
2. Perform actions related to the feature
3. Verify the expected behavior
4. Take a screenshot showing the result
5. Report: **PASS** if working, **FAIL** with details if not

## Test Scenarios

Based on the feature, test:
- Happy path (normal usage)
- Edge cases (empty input, special characters)
- Error handling (invalid actions)
```

---

## Task 3: Use Your Command

Make sure pandora-demo is running, then in Claude Code:

```
/test-feature product listing
```

Claude will:
1. Parse `$ARGUMENTS` as "product listing"
2. Navigate to the app
3. Test product listing functionality
4. Take screenshots
5. Report results

Try other features:

```
/test-feature add to basket
```

```
/test-feature search functionality
```

---

## How Arguments Work

```
/test-feature add to basket
      │            │
      │            └── $ARGUMENTS = "add to basket"
      └── Runs test-feature.md prompt
```

The `$ARGUMENTS` placeholder gets replaced with whatever you type after the command name.

---

## Task 4: Create a Quick Check Command

Create `.claude/commands/quick-check.md`:

```markdown
---
description: Quick health check of the app
---

Perform a quick health check of http://localhost:4210:

1. Navigate to the homepage
2. Verify it loads without errors
3. Check that products are displayed
4. Take a screenshot
5. Report: App is UP or DOWN with details
```

Use it:

```
/quick-check
```

This command has no arguments — it just runs the fixed prompt.

---

## Commands vs Skills: When to Use Each

| Use Case | Choice | Why |
|----------|--------|-----|
| Testing specific feature | **Command** | Explicit, with arguments |
| Creating Jira tickets | **Skill** | Auto-invoked by intent |
| Running health check | **Command** | Quick, explicit action |
| Planning tickets | **Skill** | Domain expertise needed |
| Code review | **Skill** | Complex expertise |
| Deploy workflow | **Command** | Explicit action |

**Rule of thumb:**
- **Commands** = Explicit actions you want to trigger
- **Skills** = Domain knowledge Claude applies automatically

---

## Your Structure

```
.claude/
├── commands/
│   ├── test-feature.md      # Test with arguments
│   └── quick-check.md       # Simple health check
└── ...
```

---

## Checkpoint

Before continuing, verify:

- [ ] Created `.claude/commands/` directory
- [ ] Created `test-feature.md` command
- [ ] Successfully ran `/test-feature` with arguments
- [ ] Understand the difference between commands and skills

---

## What You've Learned

1. **Slash commands** are reusable prompts invoked with `/name`
2. **`$ARGUMENTS`** captures user input after the command
3. **Frontmatter** configures description, tools, model
4. **Commands are explicit**; skills are auto-matched

---

## Next Up

Continue to: [03_custom_agents.md](./03_custom_agents.md) - Build specialized agents
