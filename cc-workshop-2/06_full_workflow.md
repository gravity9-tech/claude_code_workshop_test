# Workshop 06: Full Workflow Demonstration

**Duration: ~15 minutes**

## What You'll Learn

- See the complete Workshop 1 → Workshop 2 evolution
- Run the full orchestrated workflow end-to-end
- Verify all components working together
- Understand the final architecture

---

## The Complete Evolution

**Workshop 1 Stack**:
```
Skills → Slash Command (sequential execution)
```

**Workshop 2 Stack**:
```
Skills → Injected into Agents → Slash Command (parallel) → Hooks
```

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                      USER TRIGGER                            │
│                 /deploy-ticket "Add feature"                 │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                   HOOKS (PreToolUse)                         │
│                   • Validate inputs                          │
│                   • Log start                                │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                   SLASH COMMAND                              │
│                  /deploy-ticket                              │
│              (orchestrates agents)                           │
└─────────────────────────┬───────────────────────────────────┘
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
┌─────────────────┐             ┌─────────────────┐
│    DEV AGENT    │             │    QA AGENT     │
│                 │             │                 │
│ Injected skills:│             │ Injected skill: │
│ • create-ticket │             │ • qa-ticket     │
│ • expand-ticket │             │                 │
│ • implement-    │             │ (from Workshop 1)│
│   ticket        │             │                 │
│                 │             └────────┬────────┘
│ (from Workshop 1)│                     │
└────────┬────────┘                      │
         │         (parallel)            │
         └───────────────┬───────────────┘
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                   HOOKS (PostToolUse)                        │
│                   • Log completion                           │
│                   • Send notifications                       │
└─────────────────────────────────────────────────────────────┘
```

---

## Final Project Structure

After completing Workshop 2:

```
tea-store-demo/
├── .claude/
│   ├── agents/                      # NEW in Workshop 2
│   │   ├── code-reviewer.md         # Standalone reviewer
│   │   ├── dev-agent.md             # Injects create/expand/implement
│   │   └── qa-agent.md              # Injects qa-ticket
│   │
│   ├── commands/
│   │   └── deploy-ticket.md         # UPDATED for agent orchestration
│   │
│   ├── hooks/                       # NEW in Workshop 2
│   │   ├── pre-commit-check.sh      # Validates commits
│   │   ├── log-activity.sh          # Logs all tool use
│   │   └── notify-complete.sh       # Notifications
│   │
│   ├── skills/                      # Meta-skills + Workshop 1 skills
│   │   ├── skill-creator/           # Meta-skill: creates skills
│   │   ├── command-creator/         # Meta-skill: creates commands
│   │   ├── agent-creator/           # Meta-skill: creates agents
│   │   ├── create-ticket/           # → injected into dev-agent
│   │   ├── expand-ticket/           # → injected into dev-agent
│   │   ├── implement-ticket/        # → injected into dev-agent
│   │   └── qa-ticket/               # → injected into qa-agent
│   │
│   ├── settings.json                # NEW: hook configuration
│   └── logs/                        # NEW: activity logs
│       └── activity.log
│
├── .mcp.json                        # Jira MCP config
└── CLAUDE.md                        # Project memory
```

---

## The Journey: Workshop 1 → Workshop 2

| Component | Workshop 1 | Workshop 2 |
|-----------|-----------|------------|
| **Skills** | Created 4 lifecycle skills | Same skills, now injected into agents |
| **Agents** | N/A | dev-agent, qa-agent, code-reviewer |
| **Commands** | Sequential skill execution | Parallel agent orchestration |
| **Hooks** | N/A | Pre/post automation |
| **Context** | Main window fills up | Isolated per agent |
| **Execution** | Sequential | Parallel |

---

## Complete Workflow Demo

### Step 1: Verify All Components

**Check skills:**
```
/skills
```
Should show: skill-creator, command-creator, create-ticket, expand-ticket, implement-ticket, qa-ticket

**Check agents:**
```
/agents
```
Should show: Explore, Plan, general-purpose, code-reviewer, dev-agent, qa-agent

**Check hooks:**
```bash
cat .claude/settings.json
```
Should show PreToolUse and PostToolUse configurations

---

### Step 2: Run the Full Workflow

```
/deploy-ticket Add a product rating feature where users can rate products 1-5 stars and see average ratings
```

**Watch what happens:**

1. **Hook fires**: PreToolUse logs the start
2. **Command parses**: Recognizes this is a feature description
3. **DEV agent spawns**:
   - Creates Jira ticket with BDD acceptance criteria
   - Expands into DEV and QA subtasks
   - Implements each DEV subtask
   - Commits changes
4. **QA agent spawns** (parallel):
   - Finds QA subtasks
   - Writes Playwright tests
   - Runs tests
   - Updates subtask statuses
5. **Results aggregate**: Both agents return summaries
6. **Hook fires**: PostToolUse logs completion
7. **Summary displayed**: Final report in main context

---

### Step 3: Check the Results

**View activity log:**
```bash
cat .claude/logs/activity.log
```

**Check Jira:**
- Ticket created with BDD description
- DEV subtasks marked Done
- QA subtasks marked Done (or with test results)

**Check git log:**
```bash
git log --oneline -5
```
Should show commits for implemented subtasks.

---

## What You've Built

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│   WORKSHOP 1                    WORKSHOP 2                   │
│   (Foundation)                  (Enhancement)                │
│                                                              │
│   ┌──────────────┐             ┌──────────────┐             │
│   │    SKILLS    │ ──inject──► │    AGENTS    │             │
│   │              │             │              │             │
│   │ create-ticket│             │  dev-agent   │             │
│   │ expand-ticket│             │  qa-agent    │             │
│   │ implement-   │             │              │             │
│   │ ticket       │             └──────┬───────┘             │
│   │ qa-ticket    │                    │                     │
│   └──────────────┘                    ▼                     │
│                                ┌──────────────┐             │
│   ┌──────────────┐             │   UPDATED    │             │
│   │   COMMAND    │ ──evolve──► │   COMMAND    │             │
│   │              │             │              │             │
│   │ /deploy-     │             │ /deploy-     │             │
│   │ ticket       │             │ ticket       │             │
│   │ (sequential) │             │ (parallel)   │             │
│   └──────────────┘             └──────┬───────┘             │
│                                       │                     │
│                                       ▼                     │
│                                ┌──────────────┐             │
│                         NEW    │    HOOKS     │             │
│                                │              │             │
│                                │ pre/post     │             │
│                                │ automation   │             │
│                                └──────────────┘             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Workshop 2 Checkpoint

Confirm you've completed:

- [ ] Understand subagent architecture (isolated context, parallel execution)
- [ ] Created custom agents (code-reviewer, dev-agent, qa-agent)
- [ ] Injected Workshop 1 skills into agents
- [ ] Updated /deploy-ticket for parallel agent orchestration
- [ ] Implemented hooks for validation, logging, notifications
- [ ] Ran complete end-to-end workflow
- [ ] Verified results in Jira, git, and logs

---

## Key Takeaways

1. **Skills are the source of truth** — Define knowledge once, inject everywhere
2. **Agents provide isolation** — Parallel execution without context pollution
3. **Commands orchestrate** — Coordinate multiple agents from single entry point
4. **Hooks automate** — Event-driven actions without manual intervention
5. **Evolution, not recreation** — Build on existing work, don't start over

---

## What's Next?

Ideas for further exploration:

### More Agents
- `security-auditor` — Focused security review agent
- `performance-analyzer` — Performance profiling agent
- `documentation-writer` — Auto-generate docs agent

### More Hooks
- Slack/Teams notifications
- CI/CD pipeline triggers
- Automatic PR creation

### Cross-Project
- Share agents via `~/.claude/agents/`
- Create agent libraries for teams
- Standardize skill patterns across projects

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Agent not found | Wrong path or name | Check `.claude/agents/` and `name` field |
| Skill not injected | Wrong skill name | Verify `skills:` list in agent frontmatter |
| Hook not firing | Matcher mismatch | Check pattern in settings.json |
| Parallel not working | Sequential Task calls | Ensure agents are independent |

---

## Summary

You've transformed your Workshop 1 skills into a powerful, parallel, automated workflow:

```
                    BEFORE                          AFTER

              /deploy-ticket                  /deploy-ticket
                    │                               │
                    ▼                               ▼
    ┌───────────────────────────┐    ┌─────────────────────────┐
    │  Sequential execution     │    │  Parallel agents        │
    │  in main context          │    │  + Hooks automation     │
    │                           │    │                         │
    │  skill → skill → skill    │    │  ┌─────┐    ┌─────┐    │
    │     → skill               │    │  │ DEV │    │ QA  │    │
    │                           │    │  │agent│    │agent│    │
    │  Context fills up         │    │  └──┬──┘    └──┬──┘    │
    │  Slow execution           │    │     └────┬─────┘       │
    └───────────────────────────┘    │          ▼             │
                                     │   Clean summary        │
                                     │   Fast parallel        │
                                     └─────────────────────────┘
```

Congratulations on completing Workshop 2!
