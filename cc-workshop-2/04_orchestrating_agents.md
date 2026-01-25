# Workshop 04: Orchestrating Agents with Slash Commands

**Duration: ~15 minutes**

## What You'll Learn

- Evolution from Workshop 1's sequential execution
- How to orchestrate custom agents from slash commands
- Use command-creator to update the deploy-ticket command
- Parallel agent execution patterns

---

## Evolution from Workshop 1

**Workshop 1**: `/deploy-ticket` called skills sequentially in main context

```
/deploy-ticket
      │
      ▼ (sequential, main context)
create-ticket → expand-ticket → implement-ticket → qa-ticket
      │              │                │                │
      ▼              ▼                ▼                ▼
   context        context          context          context
   grows          grows            grows            grows
```

**Workshop 2**: `/deploy-ticket` orchestrates agents with parallel execution

```
/deploy-ticket
      │
      ├──────────────────┐
      ▼                  ▼
┌───────────┐      ┌───────────┐
│ DEV Agent │      │ QA Agent  │
│ (isolated)│      │ (isolated)│
└─────┬─────┘      └─────┬─────┘
      │   (parallel)     │
      └────────┬─────────┘
               ▼
        Summarized Results
```

---

## Benefits of Agent Orchestration

| Aspect | Sequential (W1) | Parallel Agents (W2) |
|--------|-----------------|---------------------|
| Execution | One at a time | Simultaneous |
| Context | Main window fills | Isolated per agent |
| Speed | Sum of all phases | Longest phase only |
| Results | Full detail in chat | Summarized |

---

## Task: Update deploy-ticket with Command Creator

We'll use the `command-creator` skill from Workshop 1 to update `/deploy-ticket` for agent orchestration.

### Prompt the command-creator

```
Update the existing deploy-ticket command to orchestrate agents instead of calling skills directly.

The updated command should:
- Parse input: feature description OR ticket key
- Support flags: --skip-qa, --dry-run
- Launch dev-agent for: create ticket → expand → implement
- Launch qa-agent in parallel for: testing (unless --skip-qa)
- Aggregate results from both agents
- Include error handling with recovery suggestions

The command should use the Task tool to spawn agents, enabling parallel execution.
```

### What command-creator generates

The command-creator will update `.claude/commands/deploy-ticket.md`:

```yaml
---
description: Orchestrate full dev lifecycle using parallel agents
allowed-tools: Task, Read
argument-hint: [feature-description or TICKET-KEY] [--skip-qa] [--dry-run]
disable-model-invocation: true
---

# Deploy Ticket (Agent Orchestration)

Orchestrate the complete development lifecycle using skill-injected agents.

## Arguments

- `$ARGUMENTS` - Feature description OR existing ticket key
- `--skip-qa` - Skip the QA testing phase
- `--dry-run` - Only create and expand, no implementation

## Workflow

### Phase 1: Parse Input

- If `$ARGUMENTS` matches ticket pattern (e.g., PROJECT-123): existing ticket
- Otherwise: feature description needing new ticket

### Phase 2: DEV Agent Execution

Launch **dev-agent** using Task tool:

```
Task: "Process ticket: $ARGUMENTS
      - Create ticket if description provided
      - Expand into subtasks
      - Implement DEV subtasks (unless --dry-run)
      Return: ticket key, subtasks, files changed"
Agent: dev-agent
```

### Phase 3: QA Agent Execution (Parallel)

If NOT --skip-qa and NOT --dry-run, launch **qa-agent**:

```
Task: "Test ticket [KEY from DEV agent]
      - Find QA subtasks
      - Write and run Playwright tests
      Return: tests passed/failed, subtasks completed"
Agent: qa-agent
```

### Phase 4: Aggregate Results

Combine both agent results into summary.

## Error Handling

- DEV fails: Report error, suggest `/implement-ticket KEY`
- QA fails: Report DEV success + QA failure, suggest `/qa-ticket KEY`
```

---

## Key Changes from Workshop 1

| Workshop 1 | Workshop 2 |
|-----------|------------|
| `Use the create-ticket skill...` | `Launch dev-agent with Task tool...` |
| `Use the qa-ticket skill...` | `Launch qa-agent with Task tool...` |
| Sequential in main context | Parallel in isolated contexts |
| Full details in chat | Summarized results |

---

## How Parallel Execution Works

The command instructs Claude to launch multiple agents. Claude can spawn them simultaneously using multiple Task tool calls:

```
Claude issues:

Task({
  prompt: "Implement ticket...",
  subagent_type: "dev-agent"
})

Task({
  prompt: "Test ticket...",
  subagent_type: "qa-agent"
})
```

Both agents work simultaneously in isolated contexts.

---

## Diagram: Before vs After

**Before (Workshop 1)**:
```
/deploy-ticket "Add ratings"
      │
      ▼ sequential, main context fills up
┌─────────────────────────────────────────┐
│ create → expand → implement → qa        │
│                                         │
│ All details stay in your context        │
│ Each step waits for previous            │
└─────────────────────────────────────────┘
```

**After (Workshop 2)**:
```
/deploy-ticket "Add ratings"
      │
      ▼
┌─────────────────────────────────────────┐
│           ORCHESTRATOR                   │
└─────────────┬───────────────────────────┘
              │
      ┌───────┴───────┐
      ▼               ▼
┌──────────┐    ┌──────────┐
│DEV Agent │    │QA Agent  │
│          │    │          │
│create    │    │qa-ticket │
│expand    │    │skill     │
│implement │    │          │
│skills    │    │(parallel)│
└────┬─────┘    └────┬─────┘
     │               │
     └───────┬───────┘
             ▼
      Summarized Results
      (clean main context)
```

---

## Task: Test the Updated Command

### Test 1: Full Workflow

```
/deploy-ticket Add a product wishlist feature where users can save items for later
```

Watch for:
- DEV agent spawns, creates ticket, implements
- QA agent spawns for testing
- Results aggregate into summary

### Test 2: Dry Run

```
/deploy-ticket --dry-run Add email notifications for order status
```

Watch for:
- Only ticket creation and expansion
- No implementation or testing

### Test 3: Existing Ticket

```
/deploy-ticket PROJECT-123
```

Watch for:
- Skips ticket creation
- Proceeds with expand → implement → qa

---

## The Meta-Skill Pattern Complete

You've now used all three meta-skills:

| Workshop | Meta-Skill | Created |
|----------|------------|---------|
| W1 | `skill-creator` | 4 lifecycle skills |
| W1 | `command-creator` | `/deploy-ticket` (sequential) |
| W2 | `agent-creator` | dev-agent, qa-agent |
| W2 | `command-creator` | Updated `/deploy-ticket` (parallel) |

The pattern: **Use meta-skills to generate well-structured artifacts**.

---

## Checkpoint

You've now:

- [ ] Understand the evolution from sequential to parallel
- [ ] Used command-creator to update `/deploy-ticket`
- [ ] Know how parallel Task tool calls work
- [ ] Tested the updated command

---

## Next Up

Continue to [05_hooks.md](./05_hooks.md) to add event-driven automation with hooks.
