# Workshop 06: Creating the Code Reviewer Agent

**Duration: ~10 minutes**

## What You'll Learn

- What the code reviewer agent does in the development lifecycle
- How it acts as a quality gate between implementation and completion
- How to create a read-only agent that validates work

---

## The Code Reviewer's Role

The code reviewer is the third and final step in the lifecycle. It checks the tdd-guide's work against the original plan:

```
        ┌──────────────┐
        │   planner    │
        └──────┬───────┘
               │ plan
               ▼
        ┌──────────────┐
        │  tdd-guide   │
        └──────┬───────┘
               │ implementation
               ▼
        ┌──────────────┐
        │code-reviewer │  ◄── You are here
        └──────────────┘
```

**Input:** The original plan and the resulting implementation (code + tests)

**Output:** A structured review with findings, issues, and a verdict (pass/fail)

The code reviewer doesn't fix anything — it only reads and evaluates. Like the planner, it's a **read-only** agent. If issues are found, they're reported back for a human to decide on next steps.

---

## What It Checks

The code reviewer validates four areas:

```
┌─────────────────────────────────────────────────┐
│                Code Review Checklist             │
├─────────────────────────────────────────────────┤
│                                                  │
│  1. Plan Compliance                              │
│     □ Each plan step has been addressed          │
│     □ Acceptance criteria are met                │
│     □ No steps were skipped                      │
│                                                  │
│  2. TDD Discipline                               │
│     □ Tests exist for each piece of new behavior │
│     □ Tests are meaningful (not just passing)     │
│     □ Tests run and pass                         │
│                                                  │
│  3. Code Quality                                 │
│     □ Follows existing project patterns          │
│     □ Clear naming and structure                 │
│     □ No over-engineering or unnecessary code    │
│                                                  │
│  4. Potential Issues                             │
│     □ No obvious security concerns               │
│     □ Edge cases considered                      │
│     □ No breaking changes to existing behavior   │
│                                                  │
└─────────────────────────────────────────────────┘
```

The reviewer doesn't need TDD methodology knowledge (no skill injection). It just needs to verify that TDD *was followed* — tests exist, tests pass, and code matches the plan.

---

## Create the Agent

Use the `/agent-creator` to generate the code-reviewer agent. In your Claude Code session, type:

```
/agent-creator Create an agent called "code-reviewer"

The agent should:
- Take two inputs: the original implementation plan and a description
  of what was implemented
- Review the code changes against the plan
- Produce a structured review with findings

The review should check:

1. Plan compliance:
   - Was each step in the plan addressed?
   - Are the acceptance criteria met?
   - Were any steps skipped or only partially done?

2. TDD discipline:
   - Do tests exist for each new behavior?
   - Are the tests meaningful (testing real behavior, not trivial)?
   - Do all tests pass when run?

3. Code quality:
   - Does the code follow existing project patterns and conventions?
   - Is naming clear and consistent?
   - Is there any unnecessary complexity or over-engineering?

4. Potential issues:
   - Any security concerns (injection, XSS, exposed secrets)?
   - Edge cases that aren't handled?
   - Breaking changes to existing functionality?

Important context about the project:
- Frontend: React with TypeScript, Vite, Tailwind CSS
- Frontend tests: Vitest + React Testing Library
- Backend: FastAPI with Python, Pydantic
- Backend tests: Pytest
- E2E tests: Playwright
- Test runner: ./test.sh

The agent should:
- Be read-only (review only, never modify files)
- Run the test suite to verify tests pass
- Output a structured review with:
  - Summary of findings per category
  - List of issues (if any), each with severity (critical, warning, note)
  - A final verdict: PASS, PASS WITH NOTES, or FAIL
```

---

## Review the Generated Agent

Once the `/agent-creator` finishes, review what was generated:

```bash
# macOS/Linux
cat .claude/agents/code-reviewer.md

# Windows
type .claude\agents\code-reviewer.md
```

Verify the generated agent includes:

**Frontmatter:**
- `name: code-reviewer`
- `tools:` should be read-only plus Bash for running tests — `Read, Glob, Grep, Bash`
- `model:` should be `sonnet` or `opus` (reviewing benefits from strong reasoning)
- No `skills:` — the reviewer checks that TDD was followed, it doesn't need the methodology itself

**Body:**
- Process for reading the plan and comparing against the implementation
- The four review categories (plan compliance, TDD discipline, code quality, issues)
- Structured output format with severity levels and verdict

The structure should look something like:

```yaml
---
name: code-reviewer
description: Reviews code against an implementation plan. Use when you need
  to validate that code matches a plan and follows TDD standards.
tools: Read, Glob, Grep, Bash
model: sonnet
---

# Code Reviewer Agent

## Purpose
Review implementations against their plans and TDD standards.

## Process
1. Read the implementation plan
2. Identify which files were created or modified
3. Review each change against the plan's acceptance criteria
4. Check for test coverage and TDD compliance
5. Run the test suite
6. Compile findings and issue a verdict

## Output Format
### Code Review: [Task Summary]

#### Plan Compliance
- ...

#### TDD Discipline
- ...

#### Code Quality
- ...

#### Issues
| # | Severity | Description |
|---|----------|-------------|
| 1 | warning  | ...         |

#### Verdict: PASS / PASS WITH NOTES / FAIL
```

> **Note:** This agent includes `Bash` despite being "read-only" because it needs to run `./test.sh` to verify tests pass. It doesn't use `Write` or `Edit`.

---

## Test the Agent

If you tested the tdd-guide in Module 05 (the health check timestamp plan), you can now review that work. In Claude Code:

```
Use the code-reviewer agent to review the following:

Plan:
- Add a "timestamp" field to the GET /health endpoint response
  containing the current ISO 8601 datetime

Implementation:
- A test was added in backend/tests/ checking for the timestamp field
- The health endpoint in backend/app/api/routes.py was updated to
  include a timestamp
```

The agent should:
1. Find and read the relevant test and implementation files
2. Check that the test covers the acceptance criteria
3. Run `./test.sh` to confirm tests pass
4. Return a structured review with a verdict

---

## All Three Agents Complete

```
Setup → Concept Introduction
              │
              ▼
      ┌───────────────┐
      │  tdd-workflow  │  ← Skill (knowledge layer) ✅
      │    (skill)     │
      └───────┬───────┘
              │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
┌────────┐ ┌─────────┐ ┌─────────────┐
│planner │ │tdd-guide│ │code-reviewer│  ← Agents (execution layer) ✅
│  ✅    │ │  ✅     │ │     ✅      │
└───┬────┘ └───┬─────┘ └──────┬──────┘
    │          │              │
    └──────────┼──────────────┘
               ▼
      ┌────────────────┐
      │  /orchestrate   │  ← Command (orchestration layer)
      └───────┬────────┘
              ▼
        Full Workflow
```

The execution layer is complete. All three agents are built:

| Agent | Access | Skills | Role |
|-------|--------|--------|------|
| `planner` | Read-only | None | Analyzes and plans |
| `tdd-guide` | Read + Write | `tdd-workflow` | Implements with TDD |
| `code-reviewer` | Read + Bash | None | Reviews and validates |

Next, we'll tie them together with a single command.

---

## Checkpoint

- [ ] Understand the code reviewer's role (quality gate, read-only, validates work)
- [ ] Used `/agent-creator` to generate the code-reviewer agent
- [ ] Verified the agent is read-only (no Write or Edit tools)
- [ ] Verified no skills are injected (reviewer checks TDD, doesn't do TDD)
- [ ] Tested the agent with a sample review

---

## Next Up

Continue to: [07_orchestrate_command.md](./07_orchestrate_command.md) — Create the Orchestrate Command
