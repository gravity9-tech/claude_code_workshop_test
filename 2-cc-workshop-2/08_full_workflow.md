# Workshop 08: Full Workflow

**Duration: ~10 minutes**

## What You'll Learn

- How all the pieces work together end-to-end
- How to run the full development lifecycle with a single command
- How to verify the results

---

## Everything We Built

Here's a recap of every artifact created in this workshop:

| Module | Artifact | Type | Role |
|--------|----------|------|------|
| 02 | `tdd-workflow` | Skill | TDD methodology knowledge (red-green-refactor) |
| 04 | `planner` | Agent | Analyzes tasks, produces implementation plans |
| 05 | `tdd-guide` | Agent | Implements plans using TDD (injects tdd-workflow skill) |
| 06 | `code-reviewer` | Agent | Reviews implementation against plan and standards |
| 07 | `/orchestrate` | Command | Chains planner → tdd-guide → code-reviewer |

These five artifacts form a complete, automated development lifecycle:

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
      │  /orchestrate   │  ← Command (orchestration layer) ✅
      └───────┬────────┘
              ▼
        Full Workflow    ◄── You are here
```

---

## Run the Full Workflow

Time to run the complete lifecycle on a real feature. In Claude Code:

```
/orchestrate Add a product sorting feature. Users should be able to sort
the product listing by price (low to high, high to low) and by name
(A-Z, Z-A). Add a sort dropdown above the product grid.
```

This feature is large enough to touch both frontend and backend, giving you a realistic view of the full cycle.

---

## What to Watch For

As the workflow runs through each phase, pay attention to:

### Phase 1: Planner

- Did it explore the codebase and find the right files?
- Are the steps in a logical order (backend first, then frontend)?
- Are acceptance criteria specific and testable?
- Did it consider both the API and the UI changes?

### Phase 2: TDD Guide

- Did it write tests **before** code (red phase)?
- Did tests fail initially for the right reason?
- Did it write the minimum code to make tests pass (green phase)?
- Did it refactor after tests were green?
- Were both backend (Pytest) and frontend (Vitest) tests written?

### Phase 3: Code Reviewer

- Did it check each plan step against the implementation?
- Did it verify tests exist and pass?
- Did it flag any code quality issues?
- What was the final verdict?

---

## Verify the Result

After the workflow completes, verify everything manually:

### Run the Tests

```bash
node test.js
```

All tests — including the new ones — should pass.

### Check the Changes

See what files were created or modified:

```bash
git diff --stat
```

You should see changes in both `backend/` and `frontend/` directories, plus new test files.

### Run the App

```bash
# macOS/Linux
./start.sh

# Windows
start.bat
```

Open http://localhost:4321 and verify the sorting dropdown appears and works.

---

## Try Another One

Run the workflow again with a different feature to reinforce the pattern:

```
/orchestrate Add a product search bar. Users should be able to type a
search query and filter products by name in real time. Add a search
input above the product grid, next to the sort dropdown.
```

Notice how the same three-phase cycle applies regardless of the feature. The planner adapts to the new task, the tdd-guide follows TDD for the new tests, and the code reviewer validates it all.

---

## Key Takeaways

### The Three-Layer Pattern

```
Knowledge    →  Skills encode HOW to do things
                    │
Execution    →  Agents DO focused work in isolation
                    │
Orchestration → Commands CHAIN agents into workflows
```

This pattern is extensible. Each layer is independent:
- **Add a new skill** and any agent can inject it
- **Add a new agent** and any command can chain it
- **Add a new command** to create a different workflow from the same agents

### What Makes This Work

| Principle | How It's Applied |
|-----------|-----------------|
| **Separation of concerns** | Each agent has one job |
| **Isolation** | Agents run in their own context, no cross-contamination |
| **Reusability** | The tdd-workflow skill can be injected into any future agent |
| **Composability** | Agents can be chained in any order by any command |
| **Single source of truth** | TDD methodology lives in one skill file, not duplicated |

---

## Final Project Structure

```
tea-store-demo/
├── .claude/
│   ├── agents/
│   │   ├── planner.md          # Agent: breaks work into implementation steps
│   │   ├── tdd-guide.md        # Agent: TDD-driven implementation (+skill)
│   │   └── code-reviewer.md    # Agent: reviews code against plan & standards
│   ├── commands/
│   │   └── orchestrate.md      # Slash command: full lifecycle orchestrator
│   └── skills/
│       ├── skill-creator/      # Pre-built: creates domain skills
│       ├── command-creator/    # Pre-built: creates slash commands
│       ├── agent-creator/      # Pre-built: creates custom agents
│       └── tdd-workflow/       # Your skill: TDD methodology knowledge
├── .mcp.json                    # Jira MCP server config
└── CLAUDE.md                    # Project memory (via /init)
```

---

## Workshop Complete

You've built a complete, automated development lifecycle:

- [ ] Created the `tdd-workflow` skill (TDD methodology knowledge)
- [ ] Created the `planner` agent (analyzes and plans)
- [ ] Created the `tdd-guide` agent (implements with TDD)
- [ ] Created the `code-reviewer` agent (validates work)
- [ ] Created the `/orchestrate` command (chains all agents)
- [ ] Ran the full lifecycle end-to-end on a real feature
- [ ] Verified the results (tests pass, app works)

One skill. Three agents. One command. A full development lifecycle from idea to reviewed code.

---

## Next Up

Ready to extend what you've built? Continue to: [Assignment](../cc-assignment/) — Extend the Development Lifecycle
