# Workshop 04: Implement Ticket Skill

**Duration: ~10 minutes**

## What You'll Learn

- Use the `implement-ticket` skill to build features
- Understand phase-by-phase development
- See how the skill uses codebase context
- Handle the build process with idempotency

---

## The Implement-Ticket Skill

The `implement-ticket` skill builds features from your planned tickets:

1. **Reads the ticket** — Gets requirements and subtasks
2. **Identifies work** — Finds incomplete P1/P2/P3 subtasks
3. **Implements phases** — Builds each phase in order
4. **Uses codebase context** — Follows existing patterns
5. **Marks completion** — Updates subtask status

---

## Two Modes

| Mode | When Used | What It Does |
|------|-----------|--------------|
| **Implementation** | P1/P2/P3 subtasks pending | Builds new features |
| **Bug Fix** | Bug subtasks from QA | Fixes specific issues |

---

## Phase-Based Building

The skill works through phases in order:

```
┌─────────────────────────────────────────┐
│  P1: Foundation                         │
│  - Create types, models, scaffolding    │
├─────────────────────────────────────────┤
│            ▼ (marks P1 Done)            │
├─────────────────────────────────────────┤
│  P2: Core Implementation                │
│  - Build main logic, API endpoints      │
├─────────────────────────────────────────┤
│            ▼ (marks P2 Done)            │
├─────────────────────────────────────────┤
│  P3: Integration (if exists)            │
│  - Connect pieces, UI components        │
└─────────────────────────────────────────┘
```

Each phase is marked **Done** before moving to the next.

---

## Task 1: Start Implementation

Using your ticket from Workshop 1:

```
Build ticket [YOUR-TICKET-KEY]
```

Or:

```
Implement my wishlist feature
```

The skill will:
1. Fetch your ticket details
2. Check which phases are incomplete
3. Start implementing phase by phase

---

## Task 2: Watch the Build Process

You'll see output like:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1: P1: Backend - Wishlist model and API endpoints
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Reading ticket requirements...
Analyzing codebase patterns...
Creating wishlist model...
Adding API endpoints...
Running tests...

COMPLETED: CCD-43
Files: app/models.py, app/api/routes.py
```

---

## Task 3: Review the Changes

After each phase, examine what was built:

```
Show me the changes made to app/models.py
```

```
What files were modified?
```

The implementation follows your codebase patterns from CLAUDE.md.

---

## Idempotency in Action

If the build is interrupted, just run it again:

```
Build ticket CCD-42
```

The skill will:
- **Skip completed phases** — P1 already Done? Skip it
- **Resume from current phase** — Continue where you left off
- **Report what's already done**

```
ℹ️ Resuming implementation

P1: Backend setup - DONE (skipping)
P2: Frontend components - IN PROGRESS

Continuing from P2...
```

---

## How Codebase Context Helps

The skill uses your CLAUDE.md to:

| Context | How It's Used |
|---------|---------------|
| Tech stack | Chooses appropriate patterns (FastAPI, Pydantic) |
| File structure | Places files in correct directories |
| Naming conventions | Matches existing style |
| Code patterns | Follows established patterns |

This ensures new code **fits naturally** with existing code.

---

## Example Output

```
✅ Implementation Complete

CCD-42: Add wishlist feature for saving products
Status: IN PROGRESS

Subtasks Completed:
- [x] CCD-43: P1: Backend - Wishlist model and API endpoints
- [x] CCD-44: P2: Frontend - Wishlist UI components

Files Modified:
- app/models.py - Added Wishlist model
- app/api/routes.py - Added wishlist endpoints
- app/templates/wishlist.html - New wishlist page

URL: https://your-domain.atlassian.net/browse/CCD-42

Next: QA testing will verify acceptance criteria
```

---

## Bug Fix Mode

If QA testing later finds bugs, the skill handles those too:

```
Fix bugs in ticket CCD-42
```

Bug fix mode:
1. Reads bug subtasks created by QA
2. Analyzes the bug report
3. Makes minimal fixes
4. Doesn't touch unrelated code

---

## Handling Blockers

If implementation fails:

```
BLOCKED: CCD-43
Reason: Test failures in wishlist endpoint

Attempting fix (1/3)...
```

The skill will:
1. Try to fix up to 3 times
2. If still blocked, report and stop
3. You can investigate and retry

---

## Your Ticket Now

After implementation:

```
┌─────────────────────────────────────────┐
│  CCD-42: Add wishlist feature           │
├─────────────────────────────────────────┤
│  Status: In Progress                    │
│                                         │
│  Subtasks:                              │
│    [x] P1: Backend setup        DONE    │
│    [x] P2: Frontend components  DONE    │
│    [ ] QA: User adds to wishlist        │
│    [ ] QA: User views wishlist          │
│    [ ] QA: User removes item            │
└─────────────────────────────────────────┘
```

Implementation done! QA subtasks are ready for testing.

---

## Checkpoint

Before continuing, verify:

- [ ] Started `implement ticket` for your ticket
- [ ] Watched phases complete (P1, P2)
- [ ] New code was added to pandora-demo
- [ ] Subtasks marked as Done in Jira
- [ ] Understand idempotency (can resume)

---

## What You've Learned

1. **Implement-ticket** builds features phase-by-phase
2. **Codebase context** ensures consistent patterns
3. **Idempotency** allows safe resume
4. **Bug fix mode** handles QA feedback

---

## Next Up

Your feature is built! Time to test it.

Continue to: [05_qa_ticket.md](./05_qa_ticket.md) - Automated QA testing
