# Workshop 06: Building the Deploy Ticket Command

**Duration: ~10 minutes**

## What You'll Learn

- What slash commands are and how they differ from skills
- How commands orchestrate multiple skills into a single workflow
- How to use the `/command-creator` to build `/deploy-ticket`

---

## Skills vs Commands

In the previous module, you built four skills that each handle one phase of the development lifecycle. Running them manually one at a time works, but requires you to copy ticket keys between steps and remember the order.

**Slash commands** solve this. A command is a markdown file that Claude executes when you type `/command-name`. It orchestrates multiple skills into a single workflow.

| | Skills | Commands |
|---|--------|----------|
| **What they are** | Knowledge files | Orchestration scripts |
| **How invoked** | Auto-triggered by context | User types `/command-name` |
| **Format** | `SKILL.md` in a folder | `command-name.md` |
| **Location** | `.claude/skills/` | `.claude/commands/` |
| **Purpose** | Teach *how* to do one thing | *Chain* multiple things together |

---

## Command Structure

Every command is a markdown file in `.claude/commands/`:

```
.claude/commands/
└── deploy-ticket.md
```

The file has two parts:

1. **YAML frontmatter** — metadata (description, tools, argument hint)
2. **Markdown body** — workflow instructions Claude follows

```yaml
---
description: Does something useful
allowed-tools: Read, Write, Bash, Task
argument-hint: <expected arguments>
---

# Command Title

## Arguments
- `$ARGUMENTS` - What the user passes after the command name

## Workflow
1. First, do this...
2. Then, do that...
```

When a user types `/deploy-ticket Add a search feature to PROJECT`, the text after the command name becomes `$ARGUMENTS`.

---

## What `/deploy-ticket` Does

The command chains all four skills in sequence, automating the full lifecycle from idea to tested code:

```
User: /deploy-ticket Add a search feature to PROJECT
                    │
                    │  $ARGUMENTS = feature description + project
                    ▼
           ┌────────────────┐
           │ create-ticket  │  Skill 1: create Jira ticket with BDD description
           └───────┬────────┘
                   │ ticket key (e.g., PROJECT-123)
                   ▼
           ┌────────────────┐
           │ expand-ticket  │  Skill 2: break into DEV + QA subtasks
           └───────┬────────┘
                   │ subtask keys
                   ▼
           ┌────────────────┐
           │implement-ticket│  Skill 3: implement DEV subtasks → Done
           └───────┬────────┘
                   │ code implemented
                   ▼
           ┌────────────────┐
           │   qa-ticket    │  Skill 4: test QA subtasks with Playwright → Done
           └───────┬────────┘
                   │
                   ▼
           Final report to user
```

Each skill runs in sequence. The ticket key from `create-ticket` is passed to every subsequent skill.

---

## Create the Command

Use the `/command-creator` to generate the deploy-ticket command. In your Claude Code session, type:

```
/command-creator Create a slash command called "deploy-ticket"

The command should orchestrate the full development lifecycle by chaining
four skills in sequence:

1. CREATE PHASE — Use the "create-ticket" skill:
   - Pass it the feature description from $ARGUMENTS
   - Capture the created ticket key

2. EXPAND PHASE — Use the "expand-ticket" skill:
   - Pass it the ticket key from step 1
   - Capture the created subtask keys

3. IMPLEMENT PHASE — Use the "implement-ticket" skill:
   - Pass it the ticket key from step 1
   - It will find and implement all [DEV] subtasks
   - Capture progress updates

4. QA PHASE — Use the "qa-ticket" skill:
   - Pass it the ticket key from step 1
   - It will find and test all [QA] subtasks with Playwright
   - Capture test results

5. REPORT — Output a final summary containing:
   - The ticket key and URL
   - Number of DEV subtasks completed
   - Number of QA subtasks passed/failed
   - Overall status

The command should:
- Stop and report if ticket creation fails
- Continue through remaining phases even if some subtasks fail
- The argument hint should be: <feature description in project PROJECT_KEY>
```

Restart Claude Code to load the new command:

```bash
claude
```

---

## Test the Command

Try running the command with a feature. In Claude Code:

```
/deploy-ticket Add a product search feature where users can search by
product name in PROJECT_KEY
```

(Replace `PROJECT_KEY` with your actual Jira project key)

Watch the four phases execute in sequence:
1. A Jira ticket is created with BDD acceptance criteria
2. The ticket is expanded into DEV and QA subtasks
3. DEV subtasks are implemented and transitioned to Done
4. QA subtasks are tested with Playwright and transitioned to Done

The final output should include a summary of the entire lifecycle.

---

## The Complete Architecture

```
                              ┌──────────────┐
                              │   Jira MCP   │  ← Tool (capability layer) ✅
                              └──────┬───────┘
                                     │
          ┌──────────┬───────────────┼───────────────┐
          ▼          ▼               ▼               ▼
   ┌────────────┐ ┌────────────┐ ┌─────────────┐ ┌─────────┐
   │  create-   │ │  expand-   │ │ implement-  │ │   qa-   │  ← Skills
   │  ticket ✅ │ │  ticket ✅ │ │ ticket   ✅ │ │ticket ✅│    (knowledge
   │            │ │            │ │             │ │         │     layer)
   └─────┬──────┘ └─────┬──────┘ └──────┬──────┘ └────┬────┘
         │              │               │              │
         └──────────────┼───────────────┼──────────────┘
                        ▼               ▼
               ┌─────────────────────────────┐
               │     /deploy-ticket ✅       │  ← Command (orchestration)
               └─────────────────────────────┘
```

Every layer is built. One command runs the entire development lifecycle.

---

Continue to: [07_agents_intro.md](./07_agents_intro.md) — Introduction to Agents
