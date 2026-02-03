# Workshop 07: Orchestrating the Full Lifecycle with Deploy-Ticket

**Duration: ~10 minutes**

## What You'll Learn

- Create a slash command that orchestrates multiple skills together
- Understand the difference between skills and slash commands
- Run the entire dev lifecycle with a single command

---

## Overview: Skills vs Slash Commands

You've built four **skills** that each handle one phase of development. Now you'll create a **slash command** that orchestrates all of them into a single, end-to-end workflow.

### What's the Difference?

| Aspect | Skills | Slash Commands |
|--------|--------|----------------|
| Location | `.claude/skills/<name>/SKILL.md` | `.claude/commands/<name>.md` |
| Purpose | Domain expertise, context | Direct user-invocable actions |
| Invocation | Triggered contextually | Explicitly via `/<command-name>` |
| Use case | Knowledge Claude should have | Actions Claude should perform |

**Skills** provide Claude with domain knowledge and best practices that it applies contextually. **Slash commands** are explicit actions users invoke directly—perfect for orchestrating workflows.

### The Command Creator Skill

Just like we used `skill-creator` to build our four lifecycle skills, we have a `command-creator` skill specifically designed for building well-structured slash commands. It ensures proper frontmatter, orchestration patterns, and best practices.

### The Orchestrator Pattern

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           /deploy-ticket                                         │
│                                                                                  │
│   "Add a product rating feature where users can rate products 1-5 stars"        │
│                                                                                  │
│         │                                                                        │
│         ▼                                                                        │
│   ┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐             │
│   │  create-  │    │  expand-  │    │ implement-│    │    qa-    │             │
│   │  ticket   │ ──►│  ticket   │ ──►│  ticket   │ ──►│  ticket   │             │
│   └───────────┘    └───────────┘    └───────────┘    └───────────┘             │
│         │                │                │                │                    │
│         ▼                ▼                ▼                ▼                    │
│   Jira ticket      DEV & QA         Code changes      Playwright               │
│   created          subtasks         committed         tests pass               │
│                    created          DEV → Done        QA → Done                │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

With one command, you go from **idea to deployed feature**.

---

## Task 1: Create the Deploy-Ticket Slash Command

This slash command orchestrates the entire workflow by calling each skill in sequence. We'll use the `command-creator` skill to ensure it follows best practices.

**Prompt the command-creator:**

```
Create a slash command called "deploy-ticket" that orchestrates my four dev lifecycle skills (create-ticket, expand-ticket, implement-ticket, qa-ticket) into a single workflow.

The command should:
- Accept a feature description OR an existing ticket key
- Support --skip-qa and --dry-run flags
- Execute skills sequentially: create → expand → implement → qa
- Report progress after each phase
- Handle errors with resume instructions
- Provide a final summary with metrics
```

The command-creator will:
1. Research best practices for orchestrator commands
2. Design proper frontmatter (description, allowed-tools, argument-hint)
3. Structure the workflow with clear phases
4. Add error handling and output format
5. Validate against its checklist
6. Save to `.claude/commands/deploy-ticket.md`

### Understanding Command Frontmatter

The command-creator will generate proper YAML frontmatter:

```yaml
---
description: Orchestrate full dev lifecycle from feature to tested code
allowed-tools: Read, Edit, Write, Bash(git:*)
argument-hint: [feature-description or TICKET-KEY] [--skip-qa] [--dry-run]
disable-model-invocation: true
---
```

| Field | Purpose |
|-------|---------|
| `description` | Shown in `/help`, explains what command does |
| `allowed-tools` | Tools the command can use without prompting |
| `argument-hint` | Shows expected arguments in autocomplete |
| `disable-model-invocation` | Prevents Claude from auto-invoking (user must type `/deploy-ticket`) |

---

## Task 2: Review the Generated Command

Open the file in your editor or use terminal:

```bash
# macOS/Linux
cat .claude/commands/deploy-ticket.md

# Windows
type .claude\commands\deploy-ticket.md
```

**Verify the frontmatter includes:**

- [ ] `description` — Clear explanation for `/help`
- [ ] `allowed-tools` — Only necessary tools listed
- [ ] `argument-hint` — Shows expected input format
- [ ] `disable-model-invocation: true` — User-invoked only

**Verify the body includes:**

- [ ] Arguments section with `$ARGUMENTS` documented
- [ ] Sequential workflow through all four skills
- [ ] Ticket key passing between phases
- [ ] Progress reporting after each phase
- [ ] Error handling with resume instructions
- [ ] Support for optional flags (--skip-qa, --dry-run)
- [ ] Output section with final summary format

---

## Task 3: Restart and Verify

Restart Claude Code to load the new command:

```bash
/exit
claude
```

Verify all skills are available:

```
/skills
```

You should see:
- `skill-creator`
- `command-creator` (used to build the orchestrator)
- `create-ticket`
- `expand-ticket`
- `implement-ticket`
- `qa-ticket`

And verify the slash command is available by typing:

```
/deploy-ticket
```

Claude should recognize the command and prompt for input.

---

## Task 4: Test the Slash Command

**Full workflow from description:**

```
/deploy-ticket Add a wishlist feature where users can save products for later
```

Watch as Claude:
1. Creates the Jira ticket with BDD acceptance criteria
2. Expands it into DEV and QA subtasks
3. Implements each DEV subtask
4. Runs Playwright tests for each QA subtask
5. Reports the final summary

**Dry run (planning only):**

```
/deploy-ticket --dry-run Add user notifications for order updates
```

This creates and expands the ticket without implementing—useful for review before coding.

**Resume from existing ticket:**

```
/deploy-ticket PROJECT-456
```

Skips creation and runs expand → implement → qa on an existing ticket.

---

## The Complete Orchestrated Workflow

| Phase | Skill Used | What Happens |
|-------|------------|--------------|
| 1. Create | `create-ticket` | Jira ticket with BDD description |
| 2. Expand | `expand-ticket` | DEV and QA subtasks created |
| 3. Build | `implement-ticket` | Code written, DEV tasks → Done |
| 4. Test | `qa-ticket` | Playwright tests, QA tasks → Done |
| 5. Report | (built-in) | Summary with metrics |

---

## Command & Skill Composition Diagram

```
                         USER: "/deploy-ticket Add ratings feature"
                                    │
                                    ▼
                    ┌───────────────────────────────┐
                    │        deploy-ticket          │
                    │    (SLASH COMMAND ORCHESTRATOR)│
                    └───────────────┬───────────────┘
                                    │
         ┌──────────────────────────┼──────────────────────────┐
         │                          │                          │
         ▼                          ▼                          ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  create-ticket  │    │ implement-ticket│    │    qa-ticket    │
│  expand-ticket  │    │                 │    │                 │
└────────┬────────┘    └────────┬────────┘    └────────┬────────┘
         │                      │                      │
         ▼                      ▼                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                         MCP TOOLS                                │
│  ┌──────────────────────────┐  ┌──────────────────────────────┐ │
│  │        JIRA MCP          │  │       PLAYWRIGHT MCP         │ │
│  │  • create, search        │  │  • navigate, click           │ │
│  │  • get, transition       │  │  • type, snapshot            │ │
│  └──────────────────────────┘  └──────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

## Checkpoint

You've now:

- [ ] Used the `command-creator` skill to generate the orchestrator
- [ ] Created `/deploy-ticket` — A slash command with proper frontmatter
- [ ] Verified the command works with `/deploy-ticket`
- [ ] Tested with a feature description
- [ ] Understand the difference between skills and slash commands

---

## Key Takeaways

1. **Command-creator for orchestrators** — Use the `command-creator` skill to generate well-structured slash commands
2. **Frontmatter matters** — `description`, `allowed-tools`, `argument-hint`, and `disable-model-invocation` control command behavior
3. **Skills vs commands** — Skills provide domain knowledge; commands provide explicit user-invoked actions
4. **Sequential orchestration** — Commands can chain skills together in a defined workflow
5. **Error recovery** — Good orchestrators provide clear resume paths when things fail
6. **Flags for flexibility** — Options like `--dry-run` and `--skip-qa` give users control

---

## Advanced: Customization Ideas

**Add deployment step:**
```
Update the deploy-ticket command to push to a staging branch after QA passes
```

**Add Slack notifications:**
```
Update the deploy-ticket command to post a summary to Slack when the workflow completes
```

**Add approval gates:**
```
Update the deploy-ticket command to pause and ask for approval before the implement phase
```

**Add rollback capability:**
```
Create a rollback-ticket command that reverts changes from a failed deploy-ticket run
```

---

## Troubleshooting

**Command not recognized:**
- Ensure the file exists at `.claude/commands/deploy-ticket.md`
- Restart Claude Code to reload commands
- Check for syntax errors in the command file

**Command not calling skills:**
- Ensure all four prerequisite skills are loaded (`/skills`)
- Check that skill names match exactly: `create-ticket`, `expand-ticket`, `implement-ticket`, `qa-ticket`

**Workflow stops mid-execution:**
- Check which phase failed in the output
- Run the individual skill for that phase to debug
- Resume with `/deploy-ticket TICKET-KEY` to continue

**Partial completion:**
- Some subtasks may be marked Done while others failed
- Check the Jira board to see current status
- Re-run `/implement-ticket` or `/qa-ticket` for the specific ticket

---

## Summary

You've built a complete **skill-based development lifecycle** with a **slash command orchestrator**:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│   META-SKILLS              DOMAIN SKILLS         ORCHESTRATOR   │
│   (create other skills)    (dev lifecycle)       (workflow)     │
│                                                                  │
│   skill-creator ──────►    create-ticket    ─┐                  │
│   command-creator ─┐       expand-ticket    ─┼─► /deploy-ticket │
│                    │       implement-ticket ─┤                  │
│                    │       qa-ticket        ─┘                  │
│                    │                                             │
│                    └─► Creates well-structured commands          │
│                        with proper frontmatter                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

This pattern of **meta-skills + domain skills + slash command orchestrator** is powerful for any workflow automation.

---

## Next Up

Continue to **Workshop 2** for:
- Playwright MCP setup and browser automation
- Running the full workflow end-to-end
- Advanced hooks and workflow orchestration
