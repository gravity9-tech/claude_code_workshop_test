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

## Next Assignment

Continue to: [03_documentation_agent.md](./03_documentation_agent.md) — Auto-Generate Documentation (Bonus)
