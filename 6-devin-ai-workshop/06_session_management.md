# Workshop 06: Session Management & Working at Scale

**Duration: ~10 minutes**

## What You'll Learn

- How to work with Devin as an autonomous agent
- The Plan → Build → Review workflow
- How to run parallel sessions and review PRs
- Key reminders to take with you

---

## 1. Working with an Autonomous Agent

Devin is not a tool you sit with — it's a team member you delegate to. Understanding this changes how you work.

| Characteristic | What This Enables |
|---------------|-------------------|
| **Available on-demand** | Delegate tasks at any time — before a meeting, at the end of the day, on a weekend. No scheduling needed. |
| **Unlimited capacity** | Spin up multiple sessions in parallel — one per file, per component, per ticket. |
| **Handles tedious work** | Pass off grunt work. Scale repetitive tasks through Playbooks. Review PRs when you're ready. |
| **No timezone bottlenecks** | Get technical answers and working code back without waiting for someone in another timezone. |
| **Amplifies your output** | Use Devin to unblock yourself, explore approaches, and multiply your delivery capacity. |

---

## 2. The Plan → Build → Review Workflow

Every Devin task follows a three-phase cycle. You're in the loop at the start and end — Devin handles the middle.

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  PLAN                BUILD                 REVIEW               │
│  Engineer in         Devin in              Engineer back        │
│  the loop            the loop              in the loop          │
│                                                                 │
│  ┌──────────┐       ┌──────────────┐       ┌──────────────┐    │
│  │ Ask Devin│       │ Devin spins  │       │ Devin opens  │    │
│  │ / Wiki   │──────►│ up workspace │──────►│ a PR with    │    │
│  │          │       │              │       │ passing tests│    │
│  │ Clarify  │       │ Plans →      │       │              │    │
│  │ scope    │       │ Executes →   │       │ You review,  │    │
│  │          │       │ Tests →      │       │ comment, or  │    │
│  │ ~5 min   │       │ Iterates     │       │ merge        │    │
│  │ of your  │       │              │       │              │    │
│  │ time     │       │ Fully async  │       │ ~15% of the  │    │
│  │          │       │ You move on  │       │ original     │    │
│  │          │       │ to other work│       │ task effort  │    │
│  └──────────┘       └──────────────┘       └──────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Result:** One clear hand-off, 6-12x engineer leverage, zero IDE disruption.

---

## 3. Running Parallel Sessions

One of Devin's biggest advantages is that you're not limited to one task at a time. Spin up as many sessions as you need.

### Example: Three Tasks Before Lunch

```
9:00 AM  ─── Session 1: "Add name search filter to FilterSection"
         ─── Session 2: "Improve test coverage for CartContext"
         ─── Session 3: "Add sorting options to the product grid"

         You go to your standup meeting.

10:30 AM ─── Session 1: PR ready for review
         ─── Session 2: PR ready for review
         ─── Session 3: PR ready for review

         You review 3 PRs over coffee.
```

### Tips for Parallel Sessions

- **One task per session** — don't combine unrelated work
- **Scope each session tightly** — the more focused, the higher the success rate
- **Use Playbooks** for repeated task types across sessions
- **Bulk-label Jira tickets** with "Devin" to scope multiple tasks at once
- **Review PRs in batches** — let sessions finish, then review together

---

## 4. Reviewing Devin's Work

When Devin finishes a session, it opens a PR with:
- A clear title and summary of changes
- Passing tests (if you provided test commands)
- A change summary explaining what was done and why

### Review Workflow

1. **Read the PR summary** — does it match what you asked for?
2. **Check the diff** — are the changes scoped correctly?
3. **Run CI** — confirm tests and lint pass in your pipeline
4. **Comment or merge** — if something needs revision, comment on the PR and Devin can pick it up in a follow-up session

> Devin is a collaborative tool — multiple engineers can interact with and provide feedback in the same session thread.

---

## 5. Key Reminders

Take these with you as you start working with Devin day-to-day:

| Reminder | Why It Matters |
|----------|---------------|
| **Devin is a junior engineer** | Give it clear tasks, not open-ended problems. You're the architect — Devin is the builder. |
| **Asynchronous by nature** | Start a task, step away, check in later. Don't sit and watch. |
| **Tasks, not problems** | Devin works best with detailed, specific tasks. Break big problems into small tasks first. |
| **Wiki → Ask → Session** | Always research and plan before executing. This is the highest-ROI workflow. |
| **Refine your prompts** | Your first prompt won't be perfect. Review Session Insights and improve over time. |
| **Parallel is your superpower** | Run multiple sessions concurrently. One developer, many Devins. |
| **Multiplayer collaboration** | Multiple engineers can interact with Devin in the same thread — use it for handoffs and shared work. |
| **DeepWiki grows over time** | The more Devin works with your codebase, the richer its understanding becomes. |

---

## Workshop Complete

You've completed the Devin AI onboarding workshop. Here's what you covered:

| Module | Topic |
|--------|-------|
| 00 | [Introduction to Devin AI](./00_introduction.md) |
| 01 | [Getting Started](./01_getting_started.md) |
| 02 | [DeepWiki & Ask Devin](./02_deepwiki_ask_devin.md) |
| 03 | [Effective Prompting](./03_effective_prompting.md) |
| 04 | [Playbooks & Workflows](./04_playbooks_workflows.md) |
| 05 | [Integrations — Jira, GitHub & MCP](./05_integrations.md) |
| 06 | [Session Management & Working at Scale](./06_session_management.md) |

### What to Do Next

1. **Start small** — pick a low-risk task (bug fix, test coverage, documentation) and run your first real session
2. **Build a Playbook** — take a task you do repeatedly and turn it into a reusable Playbook
3. **Tag a Jira ticket** — add the "Devin" label to a real ticket and review the analysis
4. **Go parallel** — once comfortable, spin up 2-3 sessions at once and review the PRs together
5. **Share what works** — add Knowledge items and Playbooks so the whole team benefits

> The more you use Devin, the better it gets — and the better you get at using it.
