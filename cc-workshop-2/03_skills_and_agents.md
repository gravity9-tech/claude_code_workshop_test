# Workshop 03: Injecting Skills into Agents

**Duration: ~15 minutes**

## What You'll Learn

- The difference between skills and agents
- How to inject existing skills into custom agents
- Use agent-creator to build skill-injected agents
- Create dev-agent and qa-agent with Workshop 1 skills

---

## Skills vs Agents Recap

| Aspect | Skills | Agents |
|--------|--------|--------|
| **Purpose** | Domain knowledge, patterns | Task execution |
| **Context** | Loaded into main context | Isolated context |
| **Invocation** | Contextual or `/skill-name` | Via Task tool delegation |
| **Persistence** | Always available when relevant | Spawned for specific tasks |
| **Location** | `.claude/skills/` | `.claude/agents/` |

---

## The Goal: Enhance Existing Skills with Agents

Workshop 1 created four skills:
- `create-ticket` — Creates Jira tickets with BDD descriptions
- `expand-ticket` — Breaks tickets into DEV/QA subtasks
- `implement-ticket` — Implements DEV subtasks
- `qa-ticket` — Tests QA subtasks with Playwright

Now we'll **inject these into agents** using the agent-creator for:
- **Isolated context** — Each agent works without polluting main context
- **Parallel execution** — DEV and QA can run simultaneously
- **Specialized tooling** — Each agent gets only the tools it needs

---

## Skill Injection with `skills:` Frontmatter

Agents can preload skills using the `skills:` frontmatter field:

```yaml
---
name: my-agent
description: ...
skills:
  - create-ticket
  - expand-ticket
---
```

The full content of each skill is injected into the subagent's context at startup. Use the skill's `name` field (from its SKILL.md frontmatter), not the directory path.

---

## Why Inject vs Recreate?

| Approach | Pros | Cons |
|----------|------|------|
| Recreate skills in agent | Self-contained | Duplication, can drift |
| **Inject existing skills** | Single source of truth, DRY | Must list in frontmatter |

**Always inject** — Skills remain the source of truth. Update a skill once, all agents benefit.

---

## Task 1: Create the DEV Agent

Use the agent-creator to build an agent that injects your DEV workflow skills.

### Prompt the agent-creator

```
Create an agent called "dev-agent" that handles the complete DEV workflow for Jira tickets.

It should:
- Inject these existing skills: create-ticket, expand-ticket, implement-ticket
- Have tools for: reading files, writing code, editing, git operations
- Use sonnet model
- Workflow: create ticket (if needed) → expand to subtasks → implement DEV subtasks
- Output: ticket key, subtasks completed, files changed, commits made
```

### What agent-creator generates

The agent-creator will produce something like:

```yaml
---
name: dev-agent
description: Handles complete DEV workflow for Jira tickets. Use when creating tickets, expanding into subtasks, or implementing code.
tools: Read, Write, Edit, Glob, Grep, Bash(git:*)
model: sonnet
skills:
  - create-ticket
  - expand-ticket
  - implement-ticket
---

# DEV Agent

Handles the complete development workflow from ticket creation through implementation.

## Workflow

1. **If given a feature description**: Use create-ticket skill patterns
2. **Expand**: Use expand-ticket skill to create subtasks
3. **Implement**: Use implement-ticket skill for each DEV subtask
4. **Report**: Ticket key, subtasks completed, files changed

## Output Format
...
```

The `skills:` frontmatter injects the full content of each skill into the agent's context at startup.

---

## Task 2: Create the QA Agent

Use the agent-creator to build an agent that injects the qa-ticket skill.

### Prompt the agent-creator

```
Create an agent called "qa-agent" that handles QA testing for Jira tickets.

It should:
- Inject the qa-ticket skill
- Have tools for: reading files, writing tests, running Playwright
- Use sonnet model
- Workflow: find QA subtasks → write Playwright tests → run tests → update subtasks
- Output: tests passed/failed, subtasks completed, any failures
```

### What agent-creator generates

```yaml
---
name: qa-agent
description: Handles QA testing for Jira tickets using Playwright. Use when testing features or running QA subtasks.
tools: Read, Write, Edit, Glob, Grep, Bash(npx playwright:*), Bash(npm test:*)
model: sonnet
skills:
  - qa-ticket
---

# QA Agent

Handles the complete QA workflow using Playwright for automated testing.

## Workflow

1. **Find QA subtasks**: Search for [QA] subtasks
2. **Write tests**: Create Playwright tests based on acceptance criteria
3. **Run tests**: Execute and capture results
4. **Update subtasks**: Transition based on pass/fail

## Output Format
...
```

---

## Diagram: Skill Injection Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    .claude/skills/                           │
│                    (from Workshop 1)                         │
│                                                              │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐        │
│  │create-ticket │ │expand-ticket │ │implement-    │        │
│  │   SKILL.md   │ │   SKILL.md   │ │ticket SKILL  │        │
│  └──────┬───────┘ └──────┬───────┘ └──────┬───────┘        │
│         │                │                │                  │
│         └────────────────┼────────────────┘                  │
│                          │ skills: frontmatter               │
│                          ▼                                   │
│                 ┌─────────────────┐                          │
│                 │   DEV AGENT     │                          │
│                 │                 │                          │
│                 │ skills:         │                          │
│                 │  - create-ticket│                          │
│                 │  - expand-ticket│                          │
│                 │  - implement-   │                          │
│                 │    ticket       │                          │
│                 └─────────────────┘                          │
│                                                              │
│  ┌──────────────┐                                           │
│  │  qa-ticket   │                                           │
│  │   SKILL.md   │                                           │
│  └──────┬───────┘                                           │
│         │ skills: frontmatter                                │
│         ▼                                                    │
│  ┌─────────────────┐                                        │
│  │   QA AGENT      │                                        │
│  │                 │                                        │
│  │ skills:         │                                        │
│  │  - qa-ticket    │                                        │
│  └─────────────────┘                                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Task 3: Verify Agents Load

Restart Claude Code:

```bash
/exit
claude
```

Check agents:

```
/agents
```

You should see:
- Built-in: Explore, Plan, general-purpose
- Custom: code-reviewer, **dev-agent**, **qa-agent**

---

## Task 4: Test Skill Injection

Test that the dev-agent has the injected skill knowledge:

```
Use the dev-agent to explain how it would create a ticket for "Add user wishlists"
```

The agent should describe the BDD format and process from the create-ticket skill.

---

## The Meta-Skill Pattern

Notice the pattern across workshops:

| Need | Meta-Skill | Creates |
|------|------------|---------|
| Domain expertise | `skill-creator` | SKILL.md files |
| User commands | `command-creator` | Command .md files |
| Execution units | `agent-creator` | AGENT.md files |

Each meta-skill:
1. Researches best practices
2. Designs appropriate structure
3. Validates against checklist
4. Saves to correct location

---

## Benefits Realized

| Before (Workshop 1) | After (Workshop 2) |
|---------------------|-------------------|
| Skills in main context | Skills in isolated agent context |
| Sequential execution | Ready for parallel execution |
| Context fills up with details | Summarized results only |
| Single workflow path | Composable agent units |

---

## Checkpoint

You've now:

- [ ] Understand skill injection with `skills:` frontmatter
- [ ] Used agent-creator to build dev-agent with 3 injected skills
- [ ] Used agent-creator to build qa-agent with 1 injected skill
- [ ] Verified agents load with `/agents`
- [ ] Tested that skill knowledge is accessible

---

## Next Up

Continue to [04_orchestrating_agents.md](./04_orchestrating_agents.md) to update `/deploy-ticket` for parallel agent orchestration.
