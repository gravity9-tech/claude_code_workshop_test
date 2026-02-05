# Workshop 02: Building the TDD Workflow Skill

**Duration: ~10 minutes**

## What You'll Learn

- What the `tdd-workflow` skill encodes and why it exists
- Why TDD knowledge belongs in a skill rather than in agent definitions
- How to use the `/skill-creator` to build the skill

---

## Why a Skill?

In the next modules, you'll create three agents. One of them — `tdd-guide` — needs to follow TDD methodology. We could write TDD instructions directly into that agent's definition, but there's a better approach:

| Approach | Drawback |
|----------|----------|
| Hardcode TDD knowledge into the agent | Not reusable, bloats the agent definition |
| Copy TDD instructions into every agent that needs it | Duplication, drift over time |
| **Create a skill and inject it** | **Reusable, single source of truth, any agent can use it** |

By creating `tdd-workflow` as a standalone skill, any current or future agent can inject it. The knowledge lives in one place.

---

## What is TDD?

Test-Driven Development follows a three-step cycle:

```
    ┌──────────────────────────────────────┐
    │                                      │
    ▼                                      │
┌────────┐       ┌────────┐       ┌────────────┐
│  RED   │ ────► │ GREEN  │ ────► │  REFACTOR  │
│        │       │        │       │            │
│ Write  │       │ Write  │       │ Clean up   │
│ a test │       │ just   │       │ while      │
│ that   │       │ enough │       │ keeping    │
│ fails  │       │ code   │       │ tests      │
│        │       │ to pass│       │ green      │
└────────┘       └────────┘       └────────────┘
```

**Red** — Write a test for the behavior you want. Run it. It should fail because the code doesn't exist yet.

**Green** — Write the minimum code to make the test pass. Nothing more.

**Refactor** — Clean up the code (remove duplication, improve naming, simplify). Run tests after every change to confirm they still pass.

This cycle repeats for each small piece of functionality.

---

## Create the Skill

Use the `/skill-creator` to generate the `tdd-workflow` skill. In your Claude Code session, type:

```
/skill-creator Create a skill called "tdd-workflow"

The skill should encode TDD (Test-Driven Development) methodology:

- Define the red-green-refactor cycle with clear steps:
  1. RED: Write a failing test that describes the desired behavior
     - Test should be specific and focused on one behavior
     - Run the test to confirm it fails for the right reason
  2. GREEN: Write the minimum code to make the test pass
     - Do not add extra functionality beyond what the test requires
     - Run the test to confirm it passes
  3. REFACTOR: Clean up the code while keeping tests green
     - Remove duplication
     - Improve naming and structure
     - Run tests after every change

- Include guidelines for good test writing:
  - Tests should be independent and not rely on other tests
  - Use descriptive test names that explain the expected behavior
  - Follow Arrange-Act-Assert pattern
  - Test edge cases and error conditions

- Include guidelines for when to move between phases:
  - Only move from RED to GREEN when the test fails for the expected reason
  - Only move from GREEN to REFACTOR when all tests pass
  - Only start a new RED phase when refactoring is complete and tests pass

- The skill should work with any testing framework (Jest, Pytest, Playwright, etc.)
```

---

## Review the Generated Skill

Once the `/skill-creator` finishes, review what was generated:

```bash
# macOS/Linux
cat .claude/skills/tdd-workflow/SKILL.md

# Windows
type .claude\skills\tdd-workflow\SKILL.md
```

Verify the generated `SKILL.md` includes:

- **Frontmatter** with name, description, and trigger keywords
- **Red-green-refactor cycle** with concrete steps
- **Test writing guidelines** (independent tests, descriptive names, Arrange-Act-Assert)
- **Phase transition rules** (when to move from red → green → refactor)
- **Framework-agnostic** instructions (not tied to a specific test runner)

The structure should look something like:

```yaml
---
name: tdd-workflow
description: TDD methodology guide. Use when implementing features using
  test-driven development, writing tests first, or following red-green-refactor.
allowed-tools: Read, Glob, Grep, Bash, Edit, Write
---

# TDD Workflow

## Purpose
Guide test-driven development using the red-green-refactor cycle.

## Instructions
1. RED: Write a failing test...
2. GREEN: Write minimum code...
3. REFACTOR: Clean up...

## Best Practices
- ...
```

> **Note:** The exact output will vary. The `/skill-creator` researches best practices and generates content accordingly. What matters is that the core TDD cycle and guidelines are captured.

---

## How This Skill Gets Used

You won't invoke this skill directly. Instead, agents will **inject** it into their context:

```
┌─────────────────────────────────────────────┐
│              tdd-guide agent                │
│                                             │
│  ┌─────────────────────────────────┐        │
│  │  injected: tdd-workflow skill   │        │
│  │  (red-green-refactor knowledge) │        │
│  └─────────────────────────────────┘        │
│                                             │
│  Agent instructions:                        │
│  "Implement the plan using TDD..."          │
│                                             │
│  The agent now KNOWS how to do TDD          │
│  without us repeating all the steps         │
└─────────────────────────────────────────────┘
```

This is the key pattern: **skills carry knowledge, agents carry intent**. The agent says *what* to do, the skill says *how*.

---

## Verify

Confirm the skill file exists:

```bash
ls .claude/skills/tdd-workflow/SKILL.md
```

Restart Claude Code to load the new skill:

```bash
/exit
claude
```

Check that it appears in the skills list:

```
/skills
```

You should see `tdd-workflow` listed alongside your Workshop 1 skills.

---

## Architecture So Far

```
Setup → Concept Introduction
              │
              ▼
      ┌───────────────┐
      │  tdd-workflow  │  ← Skill (knowledge layer) ✅ DONE
      │    (skill)     │
      └───────┬───────┘
              │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
┌────────┐ ┌─────────┐ ┌─────────────┐
│planner │ │tdd-guide│ │code-reviewer│  ← Agents (execution layer)
│        │ │(+skill) │ │             │
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

The knowledge layer is complete. Next, we'll build the first agent that uses it.

---

## Checkpoint

- [ ] Understand why TDD knowledge belongs in a skill (reusable, single source of truth)
- [ ] Used `/skill-creator` to generate the `tdd-workflow` skill
- [ ] Reviewed the generated `SKILL.md` and confirmed it has TDD cycle + guidelines
- [ ] Verified the skill loads with `/skills`

---

## Next Up

Continue to: [03_agents_intro.md](./03_agents_intro.md) — Understanding Agents
