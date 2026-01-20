# Workshop 07: Full Workflow

**Duration: ~8 minutes**

## What You've Built

Throughout these workshops, you've learned the individual pieces:

| Workshop 1 | Workshop 2 |
|------------|------------|
| Context Window | Playwright MCP |
| CLAUDE.md | Slash Commands |
| Jira MCP | Custom Agents |
| Skills Intro | Implement Ticket |
| Create Ticket | QA Ticket |
| Plan Ticket | Hooks |

Now let's see them work together in one command.

---

## The `/build-and-qa` Command

The dev-tools plugin provides a command that orchestrates the entire workflow:

```
/build-and-qa Add a product rating feature
```

This single command:
1. **Creates** a Jira ticket (create-ticket skill)
2. **Plans** it with BDD and subtasks (plan-ticket skill)
3. **Implements** phase by phase (implement-ticket skill)
4. **Tests** with Playwright (qa-ticket skill)

---

## How It Works

```
┌──────────────────────────────────────────────────────────┐
│                    /build-and-qa                          │
├──────────────────────────────────────────────────────────┤
│                                                          │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐ │
│   │  Phase 1    │───►│  Phase 2    │───►│  Phase 3    │ │
│   │  Create     │    │  Plan       │    │  Implement  │ │
│   │  Ticket     │    │  Ticket     │    │  Ticket     │ │
│   └─────────────┘    └─────────────┘    └─────────────┘ │
│                                                │         │
│                                                ▼         │
│                                         ┌─────────────┐ │
│                                         │  Phase 4    │ │
│                                         │  QA Test    │ │
│                                         └─────────────┘ │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

Each phase uses a specialized agent with its own context window.

---

## Smart Resume

The command detects where you are and resumes:

| State | Action |
|-------|--------|
| No ticket | Start from Phase 1 (create) |
| Ticket exists, no subtasks | Start from Phase 2 (plan) |
| Has P subtasks, not done | Start from Phase 3 (implement) |
| P subtasks done | Start from Phase 4 (QA) |
| All QA passed | Report complete |

---

## Task 1: Run a New Feature End-to-End

Let's build a completely new feature in one command:

```
/build-and-qa Add product search functionality
```

Watch as it:
1. Creates a ticket with BDD scenarios
2. Plans it with P1/P2 and QA subtasks
3. Implements the search feature
4. Tests each scenario with Playwright

---

## Task 2: Resume an Existing Ticket

If you have a ticket in progress:

```
/build-and-qa CCD-42
```

The command will detect the current state and resume from there.

---

## Task 3: Observe the Agents

As `/build-and-qa` runs, notice:
- **Agent isolation** — Each phase runs in its own context
- **Progress tracking** — Todo list shows current phase
- **Jira updates** — Ticket status changes in real-time
- **Playwright testing** — Browser automation at the end

---

## Example Full Run

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1: CREATE TICKET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Checking for existing tickets...
Creating ticket with BDD scenarios...

✅ Created: CCD-50: Add product search functionality

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2: PLAN TICKET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Expanding requirements...
Creating subtasks...

✅ Planned: 2 dev subtasks, 3 QA subtasks

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 3: IMPLEMENT TICKET
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Implementing P1: Backend search endpoint...
Implementing P2: Frontend search component...

✅ Implementation complete

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 4: QA TESTING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Testing: User searches for products... PASS
Testing: Empty search shows all... PASS
Testing: No results shows message... PASS

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ BUILD AND QA COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CCD-50: Add product search functionality

Summary:
- Implementation: Complete (2 phases)
- QA Tests: 3 passed, 0 failed

Files Modified:
- app/api/routes.py
- app/templates/search.html
- static/js/search.js

URL: https://your-domain.atlassian.net/browse/CCD-50
```

---

## The Power of Orchestration

Without `/build-and-qa`:
```
Create a ticket for search → (wait)
Plan ticket CCD-50 → (wait)
Implement ticket CCD-50 → (wait for each phase)
QA test ticket CCD-50 → (wait)
```

With `/build-and-qa`:
```
/build-and-qa Add product search
→ Everything happens automatically
```

---

## When Things Go Wrong

The workflow handles issues gracefully:

| Issue | Behavior |
|-------|----------|
| Duplicate ticket | Uses existing ticket |
| Implementation fails | Reports and stops |
| QA test fails | Creates bug, reports |
| Interrupted | Resume with same command |

---

## Your Final Structure

```
pandora-demo/
├── .claude/
│   ├── commands/
│   │   ├── test-feature.md
│   │   └── quick-check.md
│   ├── agents/
│   │   └── feature-builder.md
│   └── settings.json          # Hooks
├── .mcp.json                  # Jira + Playwright MCP
├── CLAUDE.md                  # Project memory
│
│   [Your implemented features]
├── app/
│   ├── models.py             # New models
│   └── api/routes.py         # New endpoints
└── ...
```

---

## Workshop Complete!

### What You've Learned Across Both Workshops

**Workshop 1: Foundations**
- Context window management
- CLAUDE.md project memory
- Jira MCP integration
- Skills and skill invocation
- Create-ticket and plan-ticket skills

**Workshop 2: Advanced**
- Playwright MCP for browser automation
- Custom slash commands
- Custom agents with skills
- Implement-ticket skill
- QA-ticket skill with Playwright
- Hooks for automation
- Full `/build-and-qa` workflow

### The Ticket-Driven Development Cycle

```
     Idea
       │
       ▼
  ┌─────────┐
  │ Create  │───► Jira ticket with BDD
  └────┬────┘
       │
       ▼
  ┌─────────┐
  │  Plan   │───► Subtasks (P1/P2/QA)
  └────┬────┘
       │
       ▼
  ┌─────────┐
  │Implement│───► Code (phase by phase)
  └────┬────┘
       │
       ▼
  ┌─────────┐     ┌───────┐
  │   QA    │────►│ PASS  │───► Done!
  └────┬────┘     └───────┘
       │
       ▼ FAIL
  ┌─────────┐
  │Bug Fix  │───► Back to QA
  └─────────┘
```

### Key Concepts

| Concept | Purpose |
|---------|---------|
| **Skills** | Domain expertise, auto-invoked |
| **Commands** | Explicit reusable prompts |
| **Agents** | Autonomous workers, own context |
| **MCP** | External tool integration |
| **Hooks** | Event-driven automation |
| **Idempotency** | Safe to re-run, resume |

---

## What's Next?

Now that you understand the dev-tools plugin:

1. **Customize** — Create your own skills and commands
2. **Extend** — Add new MCP servers for your tools
3. **Automate** — Build hooks for your workflow
4. **Share** — Create plugins for your team

Happy building!
