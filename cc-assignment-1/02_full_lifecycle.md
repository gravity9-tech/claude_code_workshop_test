# Assignment 02: Full Lifecycle Command

**Estimated Duration: 25-35 minutes**

## Objective

Create a `/full-lifecycle` command that bridges Workshop 1 (Jira skills) with Workshop 2 (development agents) into a single end-to-end workflow.

---

## Background

You currently have two separate workflows:

**Workshop 1: `/deploy-ticket`**
```
create-ticket → expand-ticket → implement-ticket → qa-ticket
```

**Workshop 2: `/orchestrate`**
```
planner → tdd-guide → code-reviewer
```

This assignment combines them:

```
┌─────────────────────────────────────────────────────────────┐
│                    /full-lifecycle                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  JIRA PHASE (Workshop 1 Skills)                             │
│  ┌────────────┐    ┌─────────────┐                          │
│  │  create-   │ → │   expand-   │                          │
│  │  ticket    │    │   ticket    │                          │
│  └────────────┘    └─────────────┘                          │
│         │                │                                  │
│         └────────┬───────┘                                  │
│                  ▼                                          │
│  DEVELOPMENT PHASE (Workshop 2 Agents)                      │
│  ┌─────────┐   ┌───────────┐   ┌──────────────┐            │
│  │ planner │ → │ tdd-guide │ → │code-reviewer │            │
│  └─────────┘   └───────────┘   └──────────────┘            │
│                  │                                          │
│                  ▼                                          │
│  COMPLETION PHASE (Workshop 1 Skills)                       │
│  ┌─────────────────┐                                        │
│  │ implement-ticket │  (transition to Done)                 │
│  └─────────────────┘                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Requirements

Create a `/full-lifecycle` command that:

- Takes a feature description and Jira project key as arguments
- Combines Jira ticket management (Workshop 1 skills) with development automation (Workshop 2 agents)
- Handles errors appropriately (don't transition ticket if something fails)
- Produces a final report summarizing all phases

---

## Part 1: Create the Command

Create the `/full-lifecycle` command based on the requirements above.

Verify it exists at `.claude/commands/full-lifecycle.md`

---

## Part 2: Test the Command

Run the full lifecycle with a real feature:

```
/full-lifecycle Add a "recently viewed products" section to the home page
that shows the last 5 products the user viewed in PROJ
```

(Replace `PROJ` with your Jira project key)

---

## Part 3: Verify Results

After the workflow completes:

**Check Jira:**
- Main ticket exists with BDD description
- Subtasks exist (DEV and QA)
- Ticket status is Done (if review passed)

**Check Code:**
```bash
git diff --stat
```

**Run Tests:**
```bash
./test.sh
```

---

## Success Criteria

- [ ] `/full-lifecycle` command exists in `.claude/commands/`
- [ ] Command uses Workshop 1 skills: `create-ticket`, `expand-ticket`, `implement-ticket`
- [ ] Command uses Workshop 2 agents: `planner`, `tdd-guide`, `code-reviewer`
- [ ] Jira ticket is created and transitioned appropriately
- [ ] Implementation follows TDD
- [ ] Final report includes all phases
- [ ] Tested end-to-end with a real feature

---

## Next Assignment

Continue to: [03_documentation_agent.md](./03_documentation_agent.md) — Auto-Generate Documentation (Bonus)
