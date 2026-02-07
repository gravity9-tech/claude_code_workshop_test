# Workshop 04: Creating the Planner Agent

**Duration: ~10 minutes**

## What You'll Learn

- What the planner agent does in the development lifecycle
- How to use the `/agent-creator` to build a custom agent
- How to test an agent with a sample task

---

## The Planner's Role

The planner is the first step in the development lifecycle. Before any code is written or tests are created, you need a plan:

```
        ┌──────────────┐
        │   planner    │  ◄── You are here
        └──────┬───────┘
               │ plan
               ▼
        ┌──────────────┐
        │  tdd-guide   │
        └──────┬───────┘
               │ implementation
               ▼
        ┌──────────────┐
        │code-reviewer │
        └──────────────┘
```

**Input:** A task or feature description (e.g., "Add a product rating feature")

**Output:** A structured implementation plan containing:
- Ordered steps to complete the task
- Which files to create or modify
- Acceptance criteria for each step
- Technical considerations

The planner doesn't write code — it only analyzes and plans. This makes it a **read-only** agent.

---

## Create the Agent

Use the `/agent-creator` to generate the planner agent. In your Claude Code session, type:

```
/agent-creator Create an agent called "planner"

The agent should:
- Take a task or feature description as input
- Explore the codebase to understand existing patterns and architecture
- Create a structured implementation plan

The plan should include:
1. Summary of the task and what needs to change
2. Ordered list of implementation steps, where each step has:
   - A clear description of what to do
   - Which files to create or modify
   - Acceptance criteria (how to know the step is done)
3. Technical considerations (edge cases, dependencies, potential risks)
4. Testing strategy (what tests to write, which frameworks to use)

Important context about the project:
- Frontend: React with TypeScript, Vite, Tailwind CSS
- Frontend tests: Vitest + React Testing Library
- Backend: FastAPI with Python, Pydantic
- Backend tests: Pytest
- E2E tests: Playwright
- Test runner: node test.js

The agent should:
- Be read-only (only analyze, never modify files)
- Use the existing code patterns it finds as guidance for the plan
- Output the plan in a clear markdown format
- Consider both frontend and backend changes when relevant
```

Restart Claude Code to load the new agent:

```bash
claude
```

---

## Review the Generated Agent

Once the `/agent-creator` finishes, review what was generated:

```bash
.claude/agents/planner.md
```

Verify the generated `AGENT.md` includes:

**Frontmatter:**
- `name: planner`
- `tools:` should be **read-only** — `Read, Glob, Grep` (no Write, Edit, or Bash)
- `model:` should be `sonnet` or `opus` (planning benefits from stronger reasoning)
- No `skills:` needed — the planner doesn't need TDD knowledge

**Body:**
- Clear process for analyzing the codebase
- Structured output format for the plan
- References to the project's tech stack

The structure should look something like:

```yaml
---
name: planner
description: Creates implementation plans for tasks. Use when you need
  to plan a feature, break down work, or analyze what changes are needed.
tools: Read, Glob, Grep
model: sonnet
---

# Planner Agent

## Purpose
Analyze tasks and produce structured implementation plans.

## Process
1. Read and understand the task description
2. Explore the codebase for relevant files and patterns
3. Break the task into ordered implementation steps
4. Define acceptance criteria for each step
5. Output the complete plan

## Output Format
### Plan: [Task Summary]
#### Steps
1. **[Step name]**
   - Files: ...
   - Acceptance criteria: ...
...
```

> **Note:** The exact output will vary. What matters is that the agent is read-only and produces structured plans.

---

## Test the Agent

Try running the planner with a sample task. In Claude Code:

```
Use the planner subagent to generate a plan for "Add a product rating feature where users can rate products 1-5 stars and see the average rating on each product card"
```

The agent should:
1. Explore the codebase to find product-related files
2. Identify both frontend and backend changes needed
3. Return a structured plan with steps, files, and acceptance criteria

Review the output — does it make sense? Did it find the right files? Are the steps in a logical order?

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
│  ✅    │ │(+skill) │ │             │
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

---

## Next Up

Continue to: [05_tdd_guide_agent.md](./05_tdd_guide_agent.md) — Create the TDD Guide Agent
