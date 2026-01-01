# Workshop 04: Creating Custom Slash Commands

**Duration: ~12 minutes**

## What You'll Learn

- What slash commands are and how they work
- The correct file format and frontmatter
- How to use arguments with `$ARGUMENTS`
- Build a basket test command using Playwright MCP

---

## What Are Slash Commands?

Slash commands are reusable prompts stored as Markdown files. Type `/command-name` and Claude executes the prompt inside.

**Important:** Slash commands run in the main context window, meaning they share conversation history and context with your current session.

**Locations:**
- Project: `.claude/commands/`
- Personal: `~/.claude/commands/`

---

## Command File Format

```markdown
---
description: Brief description shown in command list
argument-hint: [optional-args]
allowed-tools: Tool1, Tool2
---

Your prompt here. Use $ARGUMENTS for user input.
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `description` | No | Shown in `/help` and autocomplete |
| `argument-hint` | No | Shows expected arguments |
| `allowed-tools` | No | Restrict which tools the command can use |
| `model` | No | Override the model (e.g., `claude-3-5-haiku-20241022`) |

---

## Task 1: Create the Commands Directory

```bash
mkdir -p .claude/commands
```

---

## Task 2: Create a Basket Test Command

This command uses the Playwright MCP server we set up in the previous workshop to test shopping basket functionality.

Create `.claude/commands/test-basket.md` with command:

```bash
touch .claude/commands/test-basket.md
````
and copy paste the following content:
```markdown
---
description: Test shopping basket functionality using Playwright MCP
argument-hint: <test-scenario>
---

Run shopping basket test on http://localhost:8000 using Playwright MCP.

## Test Scenario

$ARGUMENTS

## Available Scenarios

| Argument | What it tests |
|----------|---------------|
| `add item` | Add a single product, verify basket count = 1 |
| `add multiple` | Add 3 products, verify all appear in basket |
| `remove item` | Add 2 items, remove 1, verify count decreases |
| `clear basket` | Add items, remove all, verify empty state |
| `basket total` | Add items, verify price calculation |
| `full flow` | Run all scenarios in sequence |

## Instructions

1. Navigate to http://localhost:8000
2. Perform the test scenario specified
3. Use `browser_click` to interact with add/remove buttons
4. Use `browser_get_text` to verify basket count and totals
5. Take `browser_screenshot` showing the result
6. Report: **PASS** or **FAIL** with details

If scenario is `full flow` or not specified, run all scenarios and summarize in a table.
```

---

## Task 3: Run Basket Tests

Make sure the application is running in Terminal 1:

```bash
python main.py
```

In Terminal 2, start Claude Code and verify MCP:

```bash
claude
```

```
/mcp
```

**Run specific tests:**

```bash
# Test adding a single item
/test-basket add item

# Test adding multiple items
/test-basket add multiple

# Test removing an item
/test-basket remove item

# Test clearing the basket
/test-basket clear basket

# Test price calculation
/test-basket basket total

# Run all tests
/test-basket full flow
```

---

## How It Works

The `/test-basket` command combines:

1. **Slash command** - Reusable prompt with `$ARGUMENTS`
2. **Playwright MCP** - Browser automation tools
3. **Arguments** - Specify which test scenario to run

```
/test-basket add item
     │          │
     │          └── $ARGUMENTS = "add item"
     └── Runs test-basket.md prompt
```

Claude reads the command, parses the argument, and uses Playwright MCP tools (`browser_navigate`, `browser_click`, `browser_screenshot`) to execute the test.

---

## Your Structure

```
.claude/
└── commands/
    └── test-basket.md
```

---

## Checkpoint

- [ ] Created `.claude/commands/`
- [ ] Created the `test-basket.md` command with frontmatter
- [ ] Understand argument parsing with `$ARGUMENTS`
- [ ] Ran `/test-basket add item`
- [ ] Ran `/test-basket remove item`

---

## Next Up

Continue to: [05_subagents.md](./05_subagents.md) - Parallel agents
