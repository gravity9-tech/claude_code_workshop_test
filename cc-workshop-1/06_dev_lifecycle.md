# Workshop 06: Building a Dev Lifecycle Skill Suite

**Duration: ~25 minutes**

## What You'll Learn

- Use the skill-creator to build a complete development workflow
- Create interconnected skills that form an automated dev lifecycle
- Understand how skills can work together with Jira and Playwright MCP

---

## Overview: The Dev Lifecycle

In this module, you'll create four skills that automate the entire development lifecycle:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  1. create-     │     │  2. expand-     │     │  3. implement-  │     │  4. qa-         │
│     ticket      │ ──► │     ticket      │ ──► │     ticket      │ ──► │     ticket      │
│                 │     │                 │     │                 │     │                 │
│ Create ticket   │     │ Add Dev & QA    │     │ Implement Dev   │     │ Test QA tasks   │
│ with BDD desc   │     │ subtasks        │     │ subtasks → Done │     │ with Playwright │
└─────────────────┘     └─────────────────┘     └─────────────────┘     └─────────────────┘
```

Each skill handles one phase of the workflow, making them composable and reusable.

---

## Task 1: Create the Create-Ticket Skill

This skill creates a Jira ticket with a BDD-style description.

**Prompt the skill-creator:**

```
Create a skill called "create-ticket"

The skill should:
- Create a Jira ticket based on a feature description
- Check for duplicate tickets before creating
- Determine the issue type (Bug, Story, or Task) based on context
- Write descriptions using BDD format with:
  - Summary of the feature
  - Acceptance criteria in Given/When/Then format
  - Any technical notes if relevant
- Ask the user which Jira project to use if not specified
- Return the ticket key and URL when done
```

**Review the generated skill:**

```bash
cat .claude/skills/create-ticket/SKILL.md
```

Verify it includes:
- Duplicate checking with `jira_search`
- BDD format instructions
- Issue type determination logic

---

## Task 2: Create the Expand-Ticket Skill

This skill takes an existing ticket and breaks it down into Dev and QA subtasks.

**Prompt the skill-creator:**

```
Create a skill called "expand-ticket"

The skill should:
- Take a Jira ticket key as input
- Read the ticket's description and acceptance criteria
- Create subtasks organized into two categories:

  DEV SUBTASKS (implementation work):
  - Break down the implementation into logical coding tasks
  - Each subtask should be a discrete, implementable unit
  - Prefix with "[DEV]" in the summary

  QA SUBTASKS (testing work):
  - Create test tasks based on the acceptance criteria
  - Each Given/When/Then scenario should have a corresponding QA task
  - Prefix with "[QA]" in the summary
  - Include test approach in the description

- Link all subtasks to the parent ticket
- Report the created subtask keys when done
```

**Review the generated skill:**

```bash
cat .claude/skills/expand-ticket/SKILL.md
```

Verify it:
- Reads the parent ticket first
- Creates both DEV and QA subtasks
- Uses proper Jira subtask creation

---

## Task 3: Create the Implement-Ticket Skill

This skill implements the Dev subtasks and moves them to Done.

**Prompt the skill-creator:**

```
Create a skill called "implement-ticket"

The skill should:
- Take a Jira ticket key as input (parent ticket)
- Find all [DEV] subtasks linked to that ticket
- For each DEV subtask:
  1. Read the subtask description
  2. Implement the code changes described
  3. Run relevant tests to verify the changes work
  4. Transition the subtask to "Done" status
  5. Add a comment summarizing what was implemented

- Work through subtasks sequentially
- Commit changes with meaningful messages referencing the subtask key
- Report progress as each subtask is completed
- Skip subtasks that are already Done
```

**Review the generated skill:**

```bash
cat .claude/skills/implement-ticket/SKILL.md
```

Verify it:
- Filters for [DEV] subtasks
- Implements code changes
- Transitions issues to Done

---

## Task 4: Create the QA-Ticket Skill

This skill tests the QA subtasks using Playwright and moves them to Done.

**Prompt the skill-creator:**

```
Create a skill called "qa-ticket"

The skill should:
- Take a Jira ticket key as input (parent ticket)
- Find all [QA] subtasks linked to that ticket
- For each QA subtask:
  1. Read the subtask description for test requirements
  2. Write or update Playwright tests based on the acceptance criteria
  3. Run the Playwright tests
  4. If tests pass:
     - Transition the subtask to "Done"
     - Add a comment with test results
  5. If tests fail:
     - Add a comment describing the failure
     - Leave the subtask in current status

- Use the Playwright MCP for browser automation
- Report overall test results when complete
- Include screenshots for failed tests when possible
```

**Review the generated skill:**

```bash
cat .claude/skills/qa-ticket/SKILL.md
```

Verify it:
- Filters for [QA] subtasks
- Uses Playwright MCP tools
- Handles pass/fail scenarios appropriately

---

## Task 5: Restart and Verify All Skills

Restart Claude Code to load all new skills:

```bash
/exit
claude
```

Verify all four skills are available:

```
/skills
```

You should see:
- `create-ticket`
- `expand-ticket`
- `implement-ticket`
- `qa-ticket`

---

## The Complete Workflow

Now you have a complete dev lifecycle automation:

| Phase | Skill | Input | Output |
|-------|-------|-------|--------|
| 1. Plan | `create-ticket` | Feature idea | Jira ticket with BDD description |
| 2. Break Down | `expand-ticket` | Ticket key | DEV and QA subtasks |
| 3. Build | `implement-ticket` | Ticket key | Code implemented, DEV tasks Done |
| 4. Test | `qa-ticket` | Ticket key | Tests run, QA tasks Done |

---

## Example: Full Lifecycle Demo

Try running through the complete workflow:

**Step 1: Create a ticket**
```
Create a ticket for adding a product rating feature where users can rate products 1-5 stars
```

**Step 2: Expand into subtasks**
```
Expand ticket PROJECT-123 into dev and QA subtasks
```
(Replace PROJECT-123 with your actual ticket key)

**Step 3: Implement the dev tasks**
```
Implement ticket PROJECT-123
```

**Step 4: Run QA tests**
```
QA ticket PROJECT-123
```

---

## Skill Interaction Diagram

```
                     USER REQUEST
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                    SKILL LAYER                               │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌────────┐ │
│  │create-ticket│ │expand-ticket│ │implement-   │ │qa-     │ │
│  │             │ │             │ │ticket       │ │ticket  │ │
│  └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └───┬────┘ │
└─────────┼───────────────┼───────────────┼────────────┼──────┘
          │               │               │            │
          ▼               ▼               ▼            ▼
┌─────────────────────────────────────────────────────────────┐
│                     MCP TOOLS                                │
│  ┌─────────────────────────┐    ┌─────────────────────────┐ │
│  │      JIRA MCP           │    │    PLAYWRIGHT MCP       │ │
│  │  • jira_create_issue    │    │  • browser_navigate     │ │
│  │  • jira_search          │    │  • browser_click        │ │
│  │  • jira_get_issue       │    │  • browser_snapshot     │ │
│  │  • jira_transition      │    │  • browser_type         │ │
│  └─────────────────────────┘    └─────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## Checkpoint

You've now created:

- [ ] `create-ticket` — Creates tickets with BDD descriptions
- [ ] `expand-ticket` — Breaks tickets into DEV and QA subtasks
- [ ] `implement-ticket` — Implements DEV subtasks and marks Done
- [ ] `qa-ticket` — Tests QA subtasks with Playwright and marks Done
- [ ] Verified all skills load with `/skills`

---

## Key Takeaways

1. **Composable skills** — Each skill handles one phase, making them reusable
2. **BDD-driven workflow** — Acceptance criteria flows from ticket to tests
3. **Automated transitions** — Skills move tickets through your workflow
4. **MCP integration** — Skills leverage both Jira and Playwright tools
5. **Skill generator** — Rapidly create consistent, well-structured skills

---

## Customization Ideas

**Add Sprint Assignment:**
```
Update create-ticket skill to ask which sprint to add the ticket to
```

**Add Story Points:**
```
Update expand-ticket skill to estimate story points for each subtask
```

**Add Code Review Step:**
```
Create a skill called "review-ticket" that checks code quality before marking Done
```

---

## Troubleshooting

**Subtasks not linking correctly:**
- Verify your Jira project supports subtasks
- Check the parent ticket key is correct

**Playwright tests failing:**
- Ensure Playwright MCP is configured (covered in Workshop 2)
- Verify the application is running locally

**Transition errors:**
- Your Jira workflow may have different status names
- Ask Claude to list available transitions: `What transitions are available for TICKET-123?`

---

## Next Up

Continue to [07_slash_commands.md](./07_slash_commands.md) to:
- Learn the difference between skills and slash commands
- Use the `command-creator` to build an orchestrator
- Run the entire dev lifecycle with `/deploy-ticket`
