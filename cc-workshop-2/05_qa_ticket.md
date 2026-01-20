# Workshop 05: QA Ticket Skill

**Duration: ~10 minutes**

## What You'll Learn

- Use the `qa-ticket` skill for automated testing
- See Playwright execute BDD scenarios
- Understand PASS/FAIL reporting
- How bugs are documented automatically

---

## The QA-Ticket Skill

The `qa-ticket` skill tests your implementation against BDD acceptance criteria:

1. **Reads QA subtasks** — Gets scenarios to test
2. **Parses BDD steps** — Given/When/Then
3. **Executes with Playwright** — Browser automation
4. **Records results** — Updates subtask with PASS/FAIL
5. **Documents bugs** — Creates detailed bug reports for failures

---

## Two Testing Modes

| Mode | Input | What It Tests |
|------|-------|---------------|
| **Batch** | Parent ticket | All QA: subtasks |
| **Single** | QA subtask | Just that scenario |

---

## How BDD → Playwright

```
Scenario: User adds product to wishlist
Given I am viewing a product          → browser_navigate to product
When I click "Add to wishlist"        → browser_click on button
Then the product is in my wishlist    → browser_snapshot + verify
```

The skill translates BDD steps into Playwright actions automatically.

---

## Task 1: Make Sure App is Running

Before QA, ensure pandora-demo is running:

```bash
# In a separate terminal
cd pandora-demo
make dev
# Or start separately:
# make start-backend (port 8000)
# make start-frontend (port 4210)
```

Verify frontend at: http://localhost:4210

---

## Task 2: Run QA Testing

In Claude Code:

```
QA test ticket [YOUR-TICKET-KEY]
```

Or:

```
Test my wishlist feature
```

The skill will:
1. Find all QA: subtasks
2. Test each one with Playwright
3. Record PASS or FAIL results

---

## Task 3: Watch the Tests

You'll see output like:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TESTING: CCD-45
Scenario: User adds product to wishlist
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Given: I am viewing a product
  → Navigating to http://localhost:4210
  → Verifying product page loaded

When: I click "Add to wishlist"
  → Finding wishlist button
  → Clicking button

Then: The product is in my wishlist
  → Checking wishlist indicator
  → VERIFIED: Wishlist count shows 1

RESULT: PASS ✓
Transitioning CCD-45 to Done
```

---

## PASS Results

When a test passes:

```
✅ QA Complete: CCD-42

Summary:
- QA Subtasks: 3
- Passed: 3 (transitioned to Done)
- Failed: 0
- Skipped: 0

Results:
| Subtask | Scenario | Status |
|---------|----------|--------|
| CCD-45 | User adds to wishlist | PASS ✓ |
| CCD-46 | User views wishlist | PASS ✓ |
| CCD-47 | User removes item | PASS ✓ |

All acceptance criteria verified!
```

The subtask is updated with:

```
QA Test Results
---------------
Status: PASS
Tested: 2024-01-15 14:30
Browser: Chromium (headless)

Verification:
- Given: Viewing product page
- When: Clicked "Add to wishlist"
- Then: Product appeared in wishlist

Notes:
Wishlist counter correctly updated to 1
```

---

## FAIL Results

When a test fails:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TESTING: CCD-46
Scenario: User views wishlist
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Given: I have products in my wishlist
  → Adding test product to wishlist

When: I navigate to the wishlist page
  → Navigating to /wishlist
  → ERROR: Page not found (404)

RESULT: FAIL ✗
Screenshot saved: qa-fail-CCD-46.png
```

The subtask is updated with a bug report:

```
QA Test Results
---------------
Status: FAIL
Tested: 2024-01-15 14:32
Browser: Chromium (headless)

Verification Attempted:
- Given: Products in wishlist
- When: Navigate to /wishlist
- Expected: See wishlist items
- Actual: 404 Not Found

----

BUG: Wishlist page returns 404

Severity: High

Description:
The wishlist page route is not implemented or missing.

Steps to Reproduce:
1. Add product to wishlist
2. Navigate to /wishlist
3. Page shows 404 error

Expected Result:
Wishlist page loads showing saved products

Actual Result:
404 Not Found error

Evidence:
qa-fail-CCD-46.png
```

---

## Task 4: Handle Failures

If tests fail, you have two options:

**Option 1: Fix and Retest**
```
Implement ticket CCD-42
```
(Bug fix mode will pick up the documented bug)

Then:
```
QA test ticket CCD-42
```

**Option 2: Test Single Scenario**
```
QA test CCD-46
```
(Tests just that specific subtask)

---

## Idempotency

Already-passed tests are skipped:

```
QA test ticket CCD-42
```

```
ℹ️ Some QA tests already passed

QA Subtasks:
- [x] CCD-45: User adds to wishlist - PASS (skipping)
- [ ] CCD-46: User views wishlist - Testing...
- [ ] CCD-47: User removes item - Testing...
```

To force re-run all:

```
QA test ticket CCD-42 force
```

---

## The Feedback Loop

```
┌─────────────────────────────────────────┐
│  implement-ticket                       │
│  Build the feature                      │
└──────────────────┬──────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────┐
│  qa-ticket                              │
│  Test against BDD criteria              │
└──────────────────┬──────────────────────┘
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
   ┌─────────┐          ┌─────────┐
   │  PASS   │          │  FAIL   │
   │  Done!  │          │  Bug    │
   └─────────┘          └────┬────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ implement-ticket│
                    │ (bug fix mode)  │
                    └────────┬────────┘
                             │
                             └──────► qa-ticket (retest)
```

---

## Your Ticket Status

After QA (assuming all pass):

```
┌─────────────────────────────────────────┐
│  CCD-42: Add wishlist feature           │
├─────────────────────────────────────────┤
│  Status: Done                           │
│                                         │
│  Subtasks:                              │
│    [x] P1: Backend setup        DONE    │
│    [x] P2: Frontend components  DONE    │
│    [x] QA: User adds to wishlist PASS   │
│    [x] QA: User views wishlist   PASS   │
│    [x] QA: User removes item     PASS   │
└─────────────────────────────────────────┘
```

---

## Checkpoint

Before continuing, verify:

- [ ] Ran `qa test ticket` on your ticket
- [ ] Saw Playwright execute BDD scenarios
- [ ] Understand PASS/FAIL result format
- [ ] Know how bugs are documented
- [ ] Understand the implement → qa feedback loop

---

## What You've Learned

1. **QA-ticket** automates BDD testing with Playwright
2. **Batch mode** tests all QA subtasks
3. **Single mode** tests one specific scenario
4. **FAIL results** include detailed bug reports
5. **Idempotency** skips already-passed tests

---

## Next Up

Continue to: [06_hooks.md](./06_hooks.md) - Event automation
