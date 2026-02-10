# Module 2: Key Components — MCP Tools, Skills, Agents & Slash Commands

**Duration: ~60 minutes**
**Format:** Hands-on Workshop

---

## What You'll Learn

- The differences between MCP Server tools, Skills, Subagents, and Slash Commands
- How these four component types layer together in a feature implementation lifecycle
- How to build each component type using the pre-built creator skills
- How to wire components together so agents consume skills and commands orchestrate agents

## Prerequisites

- Completed [Module 0: Setup](./00_setup.md) — Claude Code installed, Tea Store project ready
- Completed [Module 1: Context Engineering Why](./01_context_engineering_why.md)
- Jira MCP server configured and connected (verify with `/mcp`)
- Pre-built skills available: `skill-creator`, `agent-creator`, `command-creator`

---

## 1. The Four Component Types (5 min)

Claude Code's extensibility is built on four distinct component types. Each serves a different role in automating development workflows:

| | MCP Server Tools | Skills | Subagents | Slash Commands |
|---|---|---|---|---|
| **What they are** | API connections to external services | Markdown knowledge files | Isolated executors with own context | Orchestration scripts |
| **What they provide** | Capabilities (raw actions) | Knowledge (how-to guidance) | Focused execution in isolation | Workflow chaining |
| **How invoked** | Automatically by Claude when needed | Auto-triggered by context or injected into agents | Called by commands, other agents, or directly | User types `/command-name` |
| **Format** | JSON config in `.mcp.json` | `SKILL.md` in a folder | `AGENT.md` file | `command-name.md` file |
| **Location** | `.mcp.json` (project) or `~/.claude/` (personal) | `.claude/skills/` | `.claude/agents/` | `.claude/commands/` |
| **Runs in** | External process (server) | Main context window | Own isolated context window | Main context window |
| **Example** | `jira_create_issue`, `jira_search` | `create-ticket`, `tdd-workflow` | `planner`, `tdd-implementer` | `/implement-feature` |
| **Analogy** | The hands (what can be done) | The playbook (how to do it) | The specialist (does focused work) | The manager (coordinates the team) |

### How They Layer Together

```
MCP Tools provide raw capabilities
    │
    ▼
Skills teach Claude HOW to use those capabilities
    │
    ▼
Agents apply skills and tools to DO focused work in isolation
    │
    ▼
Commands CHAIN agents into end-to-end workflows
```

**Key insight:** Each layer builds on the previous. MCP tools without skills are powerful but unguided. Skills without agents run in the main context. Agents without commands require manual orchestration. Together, they form a complete automation pipeline.

---

## 2. What We're Building (5 min)

In this module, you'll build a complete feature implementation lifecycle — from Jira ticket creation to reviewed, tested code — using all four component types:

```
                              ┌──────────────────┐
                              │  Jira MCP Server │  ← MCP Tool (capability layer)
                              └────────▲─────────┘
                                  ▲    │    ▲
                           ┌──────┘    │    └──────┐
                           │           │           │
    ┌──────────┐ ┌─────────┴┐ ┌────────┴─┐ ┌───────┴────┐ ┌──────────────┐
    │ Create   │ │ Expand   │ │  TDD     │ │  Create    │ │ Commit &     │
    │ Ticket   │ │ Ticket   │ │ Workflow │ │  Branch    │ │ Merge        │ ← Skills
    │ Skill    │ │ Skill    │ │ Skill    │ │  Skill     │ │ Skill        │   (knowledge layer)
    └─────▲────┘ └────▲─────┘ └────▲─────┘ └─────▲──────┘ └──────▲───────┘
          │           │            │             │               │
          └─────┬─────┘            │             │               │
                │                  │             │               │
         ┌──────┴──────┐   ┌───────┴─────────────┴──┐   ┌────────┴───────┐
         │   Planner   │   │    TDD Implementer     │   │    Code        │
         │   Agent     │   │    Agent               │   │  Reviewer      │ ← Agents
         │             │   │                        │   │   Agent        │   (execution layer)
         └──────▲──────┘   └───────────▲────────────┘   └───────▲────────┘
                │                      │                        │
                └──────────┬───────────┴────────────────────────┘
                           │
                ┌──────────┴───────────┐
                │  /implement-feature  │  ← Slash Command (orchestration layer)
                │   Slash Command      │
                └──────────────────────┘
```

### The Components We'll Create

| # | Component | Type | Role |
|---|-----------|------|------|
| 1 | `create-ticket` | Skill | Creates a Jira ticket with BDD description via Jira MCP |
| 2 | `expand-ticket` | Skill | Breaks a ticket into DEV and QA subtasks via Jira MCP |
| 3 | `tdd-workflow` | Skill | Encodes TDD methodology (red-green-refactor) |
| 4 | `create-branch` | Skill | Creates a Git branch for the feature |
| 5 | `commit-and-merge` | Skill | Commits changes and merges with main when review passes |
| 6 | `planner` | Agent | Plans implementation, creates and expands tickets |
| 7 | `tdd-implementer` | Agent | Implements subtasks using TDD on a single feature branch |
| 8 | `code-reviewer` | Agent | Reviews code, commits and merges if no critical issues |
| 9 | `/implement-feature` | Command | Chains all three agents into one workflow |

---

## 3. Skills: The Knowledge Layer (15 min)

Skills are markdown files that teach Claude *how* to perform specific tasks. They encode procedural knowledge — instructions, best practices, and templates — so you don't have to repeat them in every prompt.

### 3.1 Create the Create-Ticket Skill

This skill teaches Claude how to create well-structured Jira tickets using the Jira MCP tools.

In your Claude Code session, type:

```
/skill-creator Create a skill called "create-ticket"

The skill should:
- Create a Jira ticket based on a feature description
- Check for duplicate tickets before creating using jira_search
- Determine the issue type (Bug, Story, or Task) based on context
- Write descriptions using BDD format with:
  - Summary of the feature
  - Acceptance criteria in Given/When/Then format
  - Any technical notes if relevant
- Ask the user which Jira project to use if not specified
- Return the ticket key and URL when done
```

**Review the generated skill:**

```bash
.claude/skills/create-ticket/SKILL.md
```

Verify it includes duplicate checking, BDD format instructions, and issue type determination logic.

### 3.2 Create the Expand-Ticket Skill

This skill takes an existing ticket and breaks it into actionable subtasks for developers and QA.

In your Claude Code session, type:

```
/skill-creator Create a skill called "expand-ticket"

The skill should:
- Take a Jira ticket key as input
- Read the ticket's description and acceptance criteria
- Create subtasks organized into two categories:

  DEV SUBTASKS (implementation work):
  - Break down the implementation into logical coding tasks
  - Each subtask should be a discrete, implementable unit
  - Prefix with "[DEV]" in the summary

  QA SUBTASKS (testing work):
  - Create test tasks based on the acceptance criteria
  - Each Given/When/Then scenario should have a corresponding QA task
  - Prefix with "[QA]" in the summary
  - Include test approach in the description

- Link all subtasks to the parent ticket
- Report the created subtask keys when done
```

**Review the generated skill:**

```bash
.claude/skills/expand-ticket/SKILL.md
```

Verify it reads the parent ticket first, creates both DEV and QA subtasks, and uses proper Jira subtask creation.

### 3.3 Create the TDD Workflow Skill

This skill encodes TDD methodology so agents can follow red-green-refactor patterns without us repeating the instructions in every agent definition.

In your Claude Code session, type:

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
     - Remove duplication, improve naming and structure
     - Run tests after every change

- Include guidelines for good test writing:
  - Tests should be independent and not rely on other tests
  - Use descriptive test names that explain the expected behavior
  - Follow Arrange-Act-Assert pattern

- The skill should work with any testing framework (Jest, Pytest, Playwright, etc.)
```

**Review the generated skill:**

```bash
.claude/skills/tdd-workflow/SKILL.md
```

### 3.4 Create the Create-Branch Skill

This skill teaches Claude to create a properly named Git branch for a feature.

In your Claude Code session, type:

```
/skill-creator Create a skill called "create-branch"

The skill should:
- Create a new Git branch for a feature being implemented
- Use the naming convention: feature/<ticket-key>-<short-description>
  (e.g., feature/PROJ-123-add-product-rating)
- Create the branch from the local main/master branch (do NOT fetch or pull from remote first)
- Switch to the new branch after creation
- Return the branch name when done
```

**Review the generated skill:**

```bash
.claude/skills/create-branch/SKILL.md
```

### Restart and Verify Skills

Restart Claude Code to load all new skills:

```bash
/exit
claude
```

Verify all four skills are available:

```
/skills
```

You should see `create-ticket`, `expand-ticket`, `tdd-workflow`, and `create-branch` listed. (The `commit-and-merge` skill will be created later in section 4.3.)

### Why Skills and Not Agent Instructions?

| Approach | Drawback |
|----------|----------|
| Hardcode knowledge into each agent | Not reusable, bloats agent definitions |
| Copy instructions into every agent | Duplication, drift over time |
| **Create a skill and inject it** | **Reusable, single source of truth, any agent can use it** |

Skills carry knowledge. Agents carry intent. This separation keeps both clean.

---

## 4. Agents: The Execution Layer (20 min)

Agents are isolated workers. Each agent gets its own context window, runs independently, and returns a result when done. This means an agent's internal work (files read, commands run, reasoning) doesn't pollute your main conversation.

```
┌─────────────────────────────────────────────┐
│          Main Context Window                │
│                                             │
│  You: "Run /implement-feature for X"        │
│                                             │
│  ┌──────────────┐  Result only              │
│  │ planner agent │ ──────────►  Plan text   │
│  │ (own context) │  returned to main        │
│  └──────────────┘                           │
│                                             │
│  All the files the agent read, the          │
│  commands it ran, its internal reasoning    │
│  — none of that pollutes the main context.  │
└─────────────────────────────────────────────┘
```

### 4.1 Create the Planner Agent

The planner is the first phase. It creates a Jira ticket, expands it into subtasks, and produces an implementation plan.

In your Claude Code session, type:

```
/agent-creator Create an agent called "planner"

The agent should:
- Take a feature description as input
- Use the "create-ticket" skill to create a Jira ticket with BDD description
- Use the "expand-ticket" skill to break the ticket into DEV and QA subtasks
- Explore the codebase to understand existing patterns and architecture
- Create a structured implementation plan containing:
  1. The Jira ticket key and summary
  2. A summary table of the parent ticket and all created subtasks
     showing: Key, Type ([DEV]/[QA]), Summary, and Status
  3. List of DEV subtasks with implementation steps
  4. Which files to create or modify for each step
  5. Acceptance criteria for each step
  6. Testing strategy

The output must include a ticket summary table like:

| Key | Type | Summary | Status |
|-----|------|---------|--------|
| PROJ-100 | Parent | Add product rating feature | To Do |
| PROJ-101 | [DEV] | Add rating API endpoint | To Do |
| PROJ-102 | [DEV] | Add rating UI component | To Do |
| PROJ-103 | [QA] | Test rating submission | To Do |

Important context about the project:
- Frontend: React with TypeScript, Vite, Tailwind CSS
- Frontend tests: Vitest + React Testing Library
- Backend: FastAPI with Python, Pydantic
- Backend tests: Pytest
- E2E tests: Playwright
- Test runner: node test.js

The agent should:
- Inject the "create-ticket" and "expand-ticket" skills
- Use the Jira MCP tools for ticket operations
- Be able to read the codebase for context (Read, Glob, Grep)
- Output the plan in clear markdown format with the ticket summary table
```

**Review the generated agent:**

```bash
.claude/agents/planner.md
```

Verify the frontmatter includes:
- `skills:` listing `create-ticket` and `expand-ticket`
- `tools:` including read access and Jira MCP tools

### 4.2 Create the TDD Implementer Agent

The TDD implementer receives the parent ticket ID, fetches task details from Jira, and implements each DEV subtask using TDD on a single feature branch. After all DEV subtasks are complete, it proceeds to implement the [QA] subtasks (e.g., writing E2E tests, integration tests). It uses Jira MCP tools to read subtask details and transition tasks as they are completed.

In your Claude Code session, type:

```
/agent-creator Create an agent called "tdd-implementer"

The agent should:
- Take a parent Jira ticket key as input (e.g., PROJ-100)
- Use the Jira MCP tools to fetch the parent ticket and all its subtasks
- Use the "create-branch" skill to create a single feature branch
  for the entire feature (not one branch per subtask)
- Inject the "tdd-workflow" skill for TDD knowledge
- Inject the "create-branch" skill for Git branching

Workflow:
1. Fetch the parent ticket details using Jira MCP (jira_get_issue)
2. Fetch all subtasks and separate them into [DEV] and [QA] subtasks
3. Create ONE feature branch using the create-branch skill
4. For each [DEV] subtask, in order:
   a. Fetch the subtask details from Jira for requirements
   b. Transition the subtask to "In Progress" on the Jira board
   c. RED: Write a failing test that covers the subtask's requirements
      - Run the test to confirm it fails for the expected reason
   d. GREEN: Write the minimum code to make the test pass
      - Run the test to confirm it passes
   e. REFACTOR: Clean up the implementation while keeping tests green
   f. Transition the subtask to "Done" on the Jira board
   g. Add a Jira comment summarizing what was implemented
5. After all [DEV] subtasks are done, proceed to [QA] subtasks.
   For each [QA] subtask, in order:
   a. Fetch the subtask details from Jira for test requirements
   b. Transition the subtask to "In Progress" on the Jira board
   c. Implement the QA tests described in the subtask
      (e.g., E2E tests with Playwright, integration tests, edge case tests)
   d. Run the tests to confirm they pass
   e. Transition the subtask to "Done" on the Jira board
   f. Add a Jira comment summarizing what was tested
6. After all subtasks ([DEV] and [QA]) are done, run the full test suite

Important context about the project:
- Frontend: React with TypeScript, Vite, Tailwind CSS
- Frontend tests: Vitest + React Testing Library (files: *.test.tsx or *.test.ts)
- Backend: FastAPI with Python, Pydantic
- Backend tests: Pytest (files: test_*.py in backend/tests/)
- E2E tests: Playwright (files: *.spec.ts in e2e/)
- Test runner: node test.js

The agent should:
- Have full write access (Read, Write, Edit, Bash, Glob, Grep)
- Have access to Jira MCP tools for reading tickets and transitioning status
- Use a single branch for the entire feature
- Implement [DEV] subtasks first, then [QA] subtasks, one by one in order
- Move each subtask through the Jira board (To Do → In Progress → Done)
- Output a summary including:
  - The feature branch name
  - Which [DEV] subtasks were implemented (with ticket keys)
  - Which [QA] subtasks were implemented (with ticket keys)
  - Which tests were written (unit/integration from DEV, E2E/QA tests from QA)
  - Which files were created or modified
  - Final test results (pass/fail counts)
```

**Review the generated agent:**

```bash
.claude/agents/tdd-implementer.md
```

Verify the frontmatter includes:
- `skills:` listing `tdd-workflow` and `create-branch`
- `tools:` including write access — `Read, Write, Edit, Bash, Glob, Grep`
- Access to Jira MCP tools for ticket operations

### 4.3 Create the Commit-and-Merge Skill

Before creating the code reviewer agent, we need one more skill. This skill handles committing changes and merging the feature branch into main — but only when the review passes without critical issues.

In your Claude Code session, type:

```
/skill-creator Create a skill called "commit-and-merge"

The skill should:
- Work entirely on local branches — do NOT push or pull to/from any remote repository
- Commit all staged and unstaged changes on the current feature branch
  with a meaningful commit message referencing the Jira ticket key
- Run the full test suite before merging to confirm all tests pass
- Merge the feature branch into the local main/master branch
- Use the following process:
  1. Stage all changes (git add)
  2. Commit with message format: "feat(TICKET-KEY): <short description>"
  3. Switch to the local main/master branch (do NOT pull from remote)
  4. Merge the feature branch into main (no fast-forward: --no-ff)
  5. Report the merge result (success or conflict)
- NEVER run git push, git pull, or git fetch — all operations are local only
- Only proceed with the merge if explicitly told the review passed
- If merge conflicts occur, report them and stop — do not auto-resolve
```

**Review the generated skill:**

```bash
.claude/skills/commit-and-merge/SKILL.md
```

### 4.4 Create the Code Reviewer Agent

The code reviewer validates the implementation against the plan and TDD standards. When no critical issues are found, it uses the `commit-and-merge` skill to commit and merge the code. It also interacts with Jira to update ticket status.

In your Claude Code session, type:

```
/agent-creator Create an agent called "code-reviewer"

The agent should:
- Take two inputs: the parent Jira ticket key and a description
  of what was implemented
- Use Jira MCP tools to fetch the ticket and subtask details
- Review the code changes against the plan and subtask requirements
- Produce a structured review checking:

  1. Plan compliance — was each step addressed? Are acceptance criteria met?
  2. TDD discipline — do tests exist? Are they meaningful? Do they pass?
  3. Code quality — follows project patterns? Clear naming? No over-engineering?
  4. Potential issues — security concerns? Edge cases? Breaking changes?

- Inject the "commit-and-merge" skill
- After the review:
  - If verdict is PASS or PASS WITH NOTES (no critical issues):
    Use the "commit-and-merge" skill to commit all changes and merge
    the feature branch into main
  - If verdict is FAIL (critical issues found):
    Do NOT commit or merge — report the issues for the developer to fix
- Update the parent Jira ticket status based on the review outcome
- Add a Jira comment with the review summary

Important context about the project:
- Frontend: React with TypeScript, Vite, Tailwind CSS
- Backend: FastAPI with Python, Pydantic
- Test runner: node test.js

The agent should:
- Be read-only for code (Read, Glob, Grep) but can run tests and git (Bash)
- Inject the "commit-and-merge" skill
- Have access to Jira MCP tools for status updates
- Run the test suite to verify tests pass
- Output a structured review with:
  - Summary of findings per category
  - List of issues with severity (critical, warning, note)
  - A final verdict: PASS, PASS WITH NOTES, or FAIL
  - Merge status (merged / not merged / conflict)
```

**Review the generated agent:**

```bash
.claude/agents/code-reviewer.md
```

### Restart and Verify Agents

Restart Claude Code to load the new agents:

```bash
/exit
claude
```

### Agent Summary

| Agent | Access | Skills Injected | Role |
|-------|--------|----------------|------|
| `planner` | Read + Jira MCP | `create-ticket`, `expand-ticket` | Plans, creates tickets, outputs summary table |
| `tdd-implementer` | Read + Write + Bash + Jira MCP | `tdd-workflow`, `create-branch` | Fetches subtasks from Jira, implements on one branch, moves tasks on the board |
| `code-reviewer` | Read + Bash + Jira MCP | `commit-and-merge` | Reviews, commits and merges if no critical issues, updates Jira |

---

## 5. Slash Command: The Orchestration Layer (10 min)

Running three agents manually — copying output between them — is tedious. A slash command chains them together into a single workflow invoked with `/implement-feature`.

### How Commands Differ

| | Skills | Agents | Commands |
|---|--------|--------|----------|
| **What they are** | Knowledge files | Isolated executors | Orchestration scripts |
| **How invoked** | Auto-triggered or injected | Called by commands or directly | User types `/command-name` |
| **Purpose** | Teach *how* | *Do* focused work | *Chain* work together |

### Create the Implement-Feature Command

In your Claude Code session, type:

```
/command-creator Create a slash command called "implement-feature"

The command should chain three agents in sequence to run a complete
feature implementation lifecycle:

1. PROJECT CHECK — Determine the Jira project:
   - Check if the user included a Jira project key in $ARGUMENTS
     (e.g., "Add login feature for PROJ")
   - If a project key is provided, use it
   - If NOT provided, use the Jira MCP tools to list all available
     Jira projects, display them to the user, and ask which project
     they want to use for creating the ticket
   - Once the project is confirmed, proceed to the next step

2. PLAN PHASE — Run the "planner" agent:
   - Pass it the feature description from $ARGUMENTS along with
     the confirmed Jira project key
   - The planner will create a Jira ticket, expand it into subtasks,
     and produce an implementation plan with a ticket summary table
   - Capture the plan output, especially the parent ticket key

3. IMPLEMENT PHASE — Run the "tdd-implementer" agent:
   - Pass it the parent Jira ticket key from step 1 (e.g., PROJ-100)
   - The implementer will fetch subtasks from Jira, create a single
     feature branch, implement each [DEV] subtask one by one using TDD,
     then implement each [QA] subtask (E2E tests, integration tests),
     moving each task on the Jira board as it progresses
   - Capture the implementation summary

4. REVIEW PHASE — Run the "code-reviewer" agent:
   - Pass it the parent Jira ticket key and the implementation summary
   - The reviewer will validate the work, run tests, and issue a verdict
   - If no critical issues: the reviewer commits and merges to main
   - If critical issues: the reviewer reports them without merging
   - Capture the review verdict and merge status

5. REPORT — Output a final summary containing:
   - The Jira ticket key and URL
   - The ticket summary table (parent + subtasks with statuses)
   - What was implemented (branch, files, tests)
   - The review verdict (PASS / PASS WITH NOTES / FAIL)
   - Merge status (merged / not merged)
   - Any issues found

The command should:
- Use the Task tool to spawn each agent as a subagent
- Run agents sequentially (each depends on the previous output)
- If the planner fails, stop and report the error
- If the implementer encounters issues, continue to review so issues are documented
- The argument hint should be: <feature description> [JIRA_PROJECT_KEY]
```

### Restart and Verify

Restart Claude Code to load the new command:

```bash
/exit
claude
```

Verify the command file exists:

```bash
.claude/commands/implement-feature.md
```

---

## 6. Test the Full Workflow (5 min)

Run the complete lifecycle with a single command:

```
/implement-feature Add a product rating feature where users can rate
products 1-5 stars and see the average rating on each product card
in PROJECT_KEY
```

(Replace `PROJECT_KEY` with your actual Jira project key)

Watch the three phases execute in sequence:

1. **Planner** — Creates a Jira ticket with BDD description, expands it into DEV/QA subtasks, outputs a plan with a ticket summary table
2. **TDD Implementer** — Receives the ticket ID, fetches subtasks from Jira, creates a single feature branch, implements each DEV subtask one by one using TDD, moves each task through the Jira board
3. **Code Reviewer** — Validates work against the plan, runs tests, issues a verdict — if no critical issues, commits and merges to main; updates Jira

---

## Architecture Recap

```
                              ┌──────────────────┐
                              │  Jira MCP Server │  ← MCP Tool (capability layer) ✅
                              └────────▲─────────┘
                                  ▲    │    ▲
                           ┌──────┘    │    └──────┐
                           │           │           │
    ┌──────────┐ ┌─────────┴┐ ┌───────┴──┐ ┌──────┴─────┐ ┌──────────────┐
    │ Create   │ │ Expand   │ │  TDD     │ │  Create    │ │ Commit &     │
    │ Ticket   │ │ Ticket   │ │ Workflow │ │  Branch    │ │ Merge        │ ← Skills
    │ Skill ✅ │ │ Skill ✅ │ │ Skill ✅ │ │  Skill ✅  │ │ Skill ✅     │   (knowledge layer) ✅
    └─────▲────┘ └────▲─────┘ └────▲─────┘ └─────▲──────┘ └──────▲──────┘
          │           │            │              │               │
          └─────┬─────┘            │              │               │
                │                  │              │               │
         ┌──────┴──────┐   ┌──────┴──────────────┴──┐   ┌───────┴────────┐
         │   Planner   │   │    TDD Implementer     │   │    Code        │
         │   Agent  ✅ │   │    Agent            ✅ │   │  Reviewer   ✅ │ ← Agents
         │             │   │                        │   │   Agent        │   (execution layer) ✅
         └──────▲──────┘   └───────────▲────────────┘   └───────▲────────┘
                │                      │                        │
                └──────────┬───────────┴────────────────────────┘
                           │
                ┌──────────┴───────────┐
                │  /implement-feature  │  ← Slash Command (orchestration layer) ✅
                │   Slash Command   ✅ │
                └──────────────────────┘
```

---

## Final Project Structure

After completing this module:

```
tea-store-demo/
├── .claude/
│   ├── agents/
│   │   ├── planner.md              # Agent: creates tickets, plans, outputs summary table
│   │   ├── tdd-implementer.md      # Agent: fetches subtasks from Jira, implements on one branch
│   │   └── code-reviewer.md        # Agent: reviews, commits & merges if no critical issues
│   ├── commands/
│   │   └── implement-feature.md    # Slash command: full lifecycle orchestrator
│   └── skills/
│       ├── skill-creator/          # Pre-built: creates domain skills
│       ├── command-creator/        # Pre-built: creates slash commands
│       ├── agent-creator/          # Pre-built: creates custom agents
│       ├── create-ticket/          # Your skill: creates Jira tickets with BDD
│       ├── expand-ticket/          # Your skill: breaks tickets into subtasks
│       ├── tdd-workflow/           # Your skill: TDD methodology knowledge
│       ├── create-branch/          # Your skill: creates feature branches
│       └── commit-and-merge/       # Your skill: commits and merges to main
├── .mcp.json                        # Jira MCP server config
└── CLAUDE.md                        # Project memory
```

---

## Key Takeaways

### The Four-Layer Pattern

| Layer | Component Type | Role | Built in This Module |
|-------|---------------|------|---------------------|
| **Capability** | MCP Server Tools | Raw actions (API calls) | Jira MCP (pre-configured) |
| **Knowledge** | Skills | How to use capabilities effectively | `create-ticket`, `expand-ticket`, `tdd-workflow`, `create-branch`, `commit-and-merge` |
| **Execution** | Agents | Do focused work in isolation | `planner`, `tdd-implementer`, `code-reviewer` |
| **Orchestration** | Slash Commands | Chain agents into workflows | `/implement-feature` |

### Why This Architecture Works

| Principle | How It's Applied |
|-----------|-----------------|
| **Separation of concerns** | Each agent has one job, each skill encodes one piece of knowledge |
| **Isolation** | Agents run in their own context — no cross-contamination |
| **Reusability** | Skills can be injected into any agent; agents can be chained by any command |
| **Composability** | Swap out an agent or skill without changing the rest of the pipeline |
| **Single source of truth** | TDD knowledge lives in one skill, not duplicated across agents |

---

## Next Up

Continue to: [03_marketplace_intro.md](./03_marketplace_intro.md) — Understanding the Marketplace
