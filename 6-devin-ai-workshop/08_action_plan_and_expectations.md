# Workshop 08: Action Plan & Expectations

**Post-Workshop Accountability Guide**

---

## 1. The Shift: From Exploration to Proving Value

The exploration phase is over. We have invested in Devin as a tool and it is time to start treating it as a team member. The expectation from this point forward is clear: **prove value against specific use cases, not broad experimentation**.

Devin is not a toy to try things with — it is a resource on your team. Assign it real work, hold it to the same standards, and measure the output.

---

## 2. Priority Use Cases

The following use cases are where we believe Devin can deliver the most impact. Each team should pick at least one and commit to validating (or disproving) it over the coming weeks.

### 2.1 Large-Scale Refactors

Give Devin the full migration plan and let it execute. Break the work into focused, parallel sessions as covered in [03_effective_prompting.md](./03_effective_prompting.md).

- Provide a clear before/after specification
- Scope each session to a single module or component
- Use Playbooks to ensure consistency across sessions

### 2.2 Modernisation & Integration Rewrites

For integrations where the behaviour also needs to change — not just a lift-and-shift but a genuine rewrite. Devin handles the tedious mapping work while you define the new contracts.

- Define the new interface/contract upfront
- Provide existing integration code as context
- Include test cases that validate the new behaviour, not just the old

### 2.3 Overnight & Background Operations

Devin runs 24/7. Use it for tasks that benefit from continuous, unattended execution:

- Root cause analysis on production incidents
- Incident preparation agents that triage logs and surface patterns
- Automated report generation and data gathering overnight

### 2.4 Version Upgrades

Framework and runtime upgrades (e.g., .NET version upgrades) are a live candidate right now. These are high-effort, well-defined tasks — ideal for Devin.

- Provide the upgrade guide as a link (Devin reads documentation)
- Break the upgrade into per-project or per-module sessions
- Include build and test commands as success criteria

### 2.5 Planning-Heavy Tickets

Sticky, complex tickets that have been sitting in the backlog because they require significant upfront analysis. Devin's planning capability (Ask Devin + session workflow) can break these down.

- Use Ask Devin to generate a structured plan first
- Review and refine the plan before launching execution sessions
- Run parallel sessions for independent subtasks

---

## 3. Remember the Best Practices

Everything we covered in this workshop applies directly to these use cases. The principles are non-negotiable:

- **Plan first, then execute** — use Ask Devin to scope work before launching sessions ([03_effective_prompting.md](./03_effective_prompting.md))
- **Be specific** — Task, Context, Requirements, Success Criteria for every session
- **Break down complex work** — multiple focused sessions beat one large session every time
- **Use Playbooks** — standardise repeated workflows across the team ([04_playbooks_workflows.md](./04_playbooks_workflows.md))
- **Include verification commands** — tests and lint as guardrails so Devin self-checks
- **Treat Devin as a team member** — assign it work, review its PRs, give it feedback through Knowledge

---

## 4. What We Expect Next Week

**Engineering managers and tech leads** are expected to come back next week with a status update covering:

1. **Which use cases** from the list above they are targeting in their area
2. **Which specific tasks or tickets** they have assigned (or plan to assign) to Devin
3. **Early results** — what worked, what did not, and what they learned
4. **Blockers** — anything preventing their team from pulling value from Devin

This is not optional. We need concrete signals on where Devin delivers and where it does not.

---

## 5. Now vs Next

We need a clear, phased approach — not everything at once.

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│   NOW — Prove the Use Cases                                         │
│   ─────────────────────────                                         │
│   - Validate the 5 priority use cases listed above                  │
│   - Measure outcomes: time saved, PR quality, ACU efficiency        │
│   - Build team confidence and Playbook library                      │
│   - Establish per-team ownership of Devin adoption                  │
│                                                                     │
│   NEXT — Integrate into the Developer Workflow                      │
│   ────────────────────────────────────────────                      │
│   - Devin moves into IDEs via Windsurf/CLI                          │
│   - Builds towards a unified workflow: Devin + Claude Code          │
│   - Teams use Devin for autonomous background work and              │
│     Claude Code for interactive, in-editor tasks                    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Prove the use cases first.** The longer-term vision of Devin integrated into the IDE alongside Claude Code only works if we have validated where Devin adds value and where it does not.

---

## 6. Defining Guardrails: Devin vs Claude Code

As Devin moves into IDEs via Windsurf/CLI, the tooling starts to look similar to Claude Code from a developer's perspective. Without clear guidance, teams will context-switch, duplicate effort, or default to whatever they opened last.

We need to define and communicate clear guardrails:

| | **Devin** | **Claude Code** |
|---|-----------|-----------------|
| **Mode** | Autonomous — runs in background | Interactive — runs in your terminal |
| **Best for** | Multi-step tasks, long-running work, parallel sessions | Quick edits, exploration, in-context questions |
| **Workflow** | Delegate and review the PR | Pair-program in real time |
| **Scope** | Whole features, migrations, upgrades | Single files, functions, debugging |
| **Availability** | 24/7, runs while you sleep | While you are actively working |
| **Think of it as** | A junior engineer on your team | A senior engineer sitting next to you |

---

## Key Takeaways

1. **Exploration is over** — start proving value against the five priority use cases
2. **Treat Devin as a team member** — assign real work, review output, build trust
3. **Plan first, then execute** — the workshop best practices are the foundation
4. **EMs and tech leads report back next week** — which use cases, which tasks, early results
5. **Now vs next** — prove the use cases first, then build towards IDE integration
6. **Define guardrails** — Devin for autonomous work, Claude Code for interactive work, no ambiguity
