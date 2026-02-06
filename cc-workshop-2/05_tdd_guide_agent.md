# Workshop 05: Creating the TDD Guide Agent

**Duration: ~10 minutes**

## What You'll Learn

- What the tdd-guide agent does in the development lifecycle
- How an agent injects a skill to gain knowledge
- How to create an agent that writes code and tests

---

## The TDD Guide's Role

The tdd-guide is the second step in the lifecycle. It takes the planner's output and turns it into working, tested code:

```
        ┌──────────────┐
        │   planner    │
        └──────┬───────┘
               │ plan
               ▼
        ┌──────────────┐
        │  tdd-guide   │  ◄── You are here
        └──────┬───────┘
               │ implementation
               ▼
        ┌──────────────┐
        │code-reviewer │
        └──────────────┘
```

**Input:** An implementation plan (from the planner agent)

**Output:** Working code with passing tests, plus a summary of what was implemented

Unlike the planner, the tdd-guide **writes code**. It creates test files, implements features, and runs the test suite. This means it needs write access to the codebase and the ability to execute commands.

---

## Skill Injection in Action

This is where the `tdd-workflow` skill you built in Module 02 comes into play. Rather than repeating TDD instructions in the agent definition, the agent simply declares the skill in its frontmatter:

```yaml
---
name: tdd-guide
skills:
  - tdd-workflow     # ← injected automatically
---
```

When the agent starts, the full contents of `tdd-workflow/SKILL.md` are loaded into its context:

```
┌────────────────────────────────────────────┐
│            tdd-guide agent context         │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │  tdd-workflow skill (injected)       │  │
│  │  • Red: write failing test           │  │
│  │  • Green: minimum code to pass       │  │
│  │  • Refactor: clean up, tests green   │  │
│  │  • Arrange-Act-Assert pattern        │  │
│  │  • Phase transition rules            │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  Agent instructions:                       │
│  "Take the plan, implement each step       │
│   using TDD..."                            │
│                                            │
│  The agent knows HOW to do TDD (skill)     │
│  and WHAT to do with it (instructions)     │
└────────────────────────────────────────────┘
```

This is the separation of concerns: the **skill** provides methodology, the **agent** provides intent.

---

## Create the Agent

Use the `/agent-creator` to generate the tdd-guide agent. In your Claude Code session, type:

```
/agent-creator Create an agent called "tdd-guide"

The agent should:
- Take an implementation plan as input (produced by the planner agent)
- Implement each step in the plan using strict TDD methodology
- Inject the "tdd-workflow" skill for TDD knowledge in frontmatter

For each step in the plan:
1. RED: Write a failing test that covers the step's acceptance criteria
   - Run the test to confirm it fails for the expected reason
2. GREEN: Write the minimum code to make the test pass
   - Run the test to confirm it passes
3. REFACTOR: Clean up the implementation while keeping tests green
   - Run tests after every change

Important context about the project:
- Frontend: React with TypeScript, Vite, Tailwind CSS
- Frontend tests: Vitest + React Testing Library (files: *.test.tsx or *.test.ts)
- Backend: FastAPI with Python, Pydantic
- Backend tests: Pytest (files: test_*.py in backend/tests/)
- E2E tests: Playwright (files: *.spec.ts in e2e/)
- Test runner: node test.js (runs both backend and frontend tests)

The agent should:
- Have full write access (it creates files and writes code)
- Follow the plan's order of steps
- Run tests after each red/green/refactor phase
- Output a summary of what was implemented, including:
  - Which tests were written
  - Which files were created or modified
  - Final test results (pass/fail counts)
```

Restart Claude Code to load the new agent:

```bash
claude
```

---

## Review the Generated Agent

Once the `/agent-creator` finishes, review what was generated:

```bash
.claude/agents/tdd-guide.md
```

Verify the generated agent includes:

**Frontmatter:**
- `name: tdd-guide`
- `tools:` should include write access — `Read, Write, Edit, Bash, Glob, Grep`
- `skills:` should list `tdd-workflow`

**Body:**
- Process that follows the plan step by step
- Explicit red-green-refactor cycle per step
- References to running tests between phases
- Output format with implementation summary

> **Key difference from the planner:** This agent has `Write`, `Edit`, and `Bash` tools — it modifies the codebase. The planner was read-only.

---

## Test the Agent

Try running the tdd-guide with a small plan. In Claude Code:

```
/tdd-guide implement the following plan:

## Plan: Add a health check timestamp

### Steps
1. **Add timestamp to health endpoint response**
   - File: backend/app/api/routes.py
   - Acceptance criteria: GET /health returns a JSON object with a
     "timestamp" field containing the current ISO 8601 datetime
```

Watch the agent's behavior — it should:
1. Write a test in `backend/tests/` that checks for the timestamp field
2. Run the test (it should fail — red)
3. Add the timestamp to the health endpoint
4. Run the test again (it should pass — green)
5. Refactor if needed
6. Return a summary

---

## Architecture So Far

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
│planner │ │tdd-guide│ │code-reviewer│  ← Agents (execution layer)
│  ✅    │ │  ✅     │ │             │
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

Two of three agents are done. One more to go.

---

## Next Up

Continue to: [06_code_reviewer_agent.md](./06_code_reviewer_agent.md) — Create the Code Reviewer Agent
