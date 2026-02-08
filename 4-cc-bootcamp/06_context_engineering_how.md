# Module 5: Context Engineering — The How

⏱️ **Duration:** ~45 minutes  
📋 **Format:** Walkthrough (20 min) + Hands-on exercise (25 min)  

---

## What You'll Learn

By the end of this module you'll have:
- A clear understanding of the four-phase context engineering process
- Knowledge of what to collect and how to make it AI-consumable
- A practical context map for your own team
- A prioritised action plan for your first two weeks post-bootcamp

---

## Why This Module Exists

Throughout today's bootcamp you've been building components — skills, agents, commands, plugins. Every one of those components is only as good as the **context behind it**.

The `tdd-workflow` skill works because it encodes TDD knowledge. The `create-ticket` skill works because it encodes your ticket conventions. The `create-branch` skill works because it encodes your branching standards.

Context engineering is the systematic process of capturing *all* of your team's knowledge — not just the bits we used today — and making it available to Claude Code. This module teaches you how to do that for your own team.

---

## Part A: The Four-Phase Process (20 min)

### Phase 1: Context Discovery

> *"What does your team already know — and where does that knowledge live?"*

This is the assessment phase. You're answering two questions:
1. Does documentation exist?
2. Is it AI-consumable?

**What to collect:**

| Category | Examples | Why Claude needs it |
|----------|---------|-------------------|
| **Coding guidelines** | Style guides, naming conventions, linting rules | Generates code that matches your standards |
| **Architecture** | System diagrams, design patterns, service boundaries | Understands where new code fits |
| **Tech stack** | Framework versions, dependencies, toolchain | Uses the right APIs and patterns |
| **Repository structure** | Mono-repo vs multi-repo, folder conventions | Knows where to create files |
| **Workflows** | PR process, deployment pipeline, incident response | Follows your team's process |
| **Testing standards** | Coverage requirements, frameworks, test naming | Writes tests your CI will accept |
| **Proprietary tech** | Internal libraries, custom frameworks, legacy APIs | Critical for teams with non-public tools |

**The documentation decision tree:**

```
Does the documentation exist?
├── YES → Is it up to date?
│   ├── YES → Gather and organise it
│   └── NO  → Update it (AI can help generate drafts)
└── NO  → Generate it
    ├── Can someone on the team write it? → Schedule a generation session
    └── Is it tribal knowledge? → Interview the expert, document it
```

**Common gaps across teams:**
- Architecture docs exist but are 2 years out of date
- Coding standards are "understood" but never written down
- Testing standards vary person to person
- Deployment process lives in one person's head

### Phase 2: Marketplace Setup

> *"Structure the context so Claude can actually use it."*

Once you've gathered the documentation, it needs to be structured in a format Claude Code can consume effectively. This means you need to identify the proper repositories and placing your context in the right folders.

**The team context structure:**

```
sample-repo/
├── claude.md                      # Always-loaded core context
│                                  # (team overview, quick reference,
│                                  #  documentation map — keep under 4KB)
│
├── context/                       # All team documentation
│   ├── coding-standards/
│   │   ├── style-guide.md
│   │   ├── review-checklist.md
│   │   └── best-practices.md
│   ├── architecture/
│   │   ├── system-overview.md
│   │   └── design-patterns.md
│   ├── tech-stack/
│   │   └── stack-inventory.md
│   ├── workflows/
│   │   ├── pr-process.md
│   │   ├── deployment-pipeline.md
│   │   └── branching-strategy.md
│   ├── testing/
│   │   └── testing-standards.md
│   ├── repositories/
│   │   └── repo-inventory.md
│   └── domain-knowledge/
│       ├── business-logic.md
│       └── glossary.md
└── ...
```

**The key principle: separate context storage from context application.**

Context lives in the `context/` folder. Skills, agents, and commands *reference* that context — they don't duplicate it. A skill's SKILL.md file should point to `{{file:../../context/testing/testing-standards.md}}` rather than copying the testing standards inline.

**Why this matters:**
- One source of truth — update the doc in one place, every skill sees the change
- No context duplication eating up the context window
- Clear ownership: documentation is maintained separately from code components

### Phase 3: Implementation Loop

> *"Build the components that make the context actionable."*

This is the iterative phase where you build skills, agents, and commands for your team. You've practised this in Modules 1–3 today.

**The implementation cadence:**

```
Identify a common team task
    → Build a skill/agent/command for it
        → Test with 2-3 team members
            → Gather feedback
                → Refine
                    → Publish to team marketplace
```

**Prioritisation guidance:**

| Priority | Build this | Because |
|----------|-----------|---------|
| **Start here** | Skills that encode your coding and testing standards | Every developer benefits immediately; highest daily impact |
| **Then** | Agents for common workflows (implementation, review, documentation) | Automates multi-step tasks developers do repeatedly |
| **Then** | Commands that chain the above into repeatable workflows | Standardises the process across the team |
| **Later** | Hooks for automatic context loading and telemetry | Adds intelligence once the basics are solid |

**The difference between "it works" and "it's reusable":**
- *It works:* The skill runs and produces output you're happy with
- *It's reusable:* Another developer on your team can install it, understand what it does from the README, and use it without asking you how — that's marketplace-ready

### Phase 4: Continuous Evolution

> *"Context isn't static. Your team evolves, and your marketplace should evolve with it."*

**What to review biweekly:**
- Are team members actually using the plugins? Which ones?
- Has anything changed in the team's standards or workflows?
- Are there new patterns or tools that need to be captured?
- Has anyone built something useful that should be shared?

**Adoption signals to watch for:**
- Developers start asking *"Is there a skill for that?"* → Context engineering is working
- Someone builds a skill without being asked → The culture is shifting
- A team member improves an existing plugin → The evolution cycle is running

**What to track:**

| Metric | How to measure | Target |
|--------|---------------|--------|
| Active Claude Code users on your team | Usage logs / self-report | all team members by Q4 |
| Plugins installed per developer | Check `.claude/` directories | 3+ per developer |
| Components contributed to marketplace | PRs to marketplace repo | 1+ per month from your team |
| Time saved on common tasks | Developer self-report | Measurable improvement in PR turnaround |

---

## Part B: Your Team's Context Map (25 min)

This is the most important exercise of the day. Everything else you built today was practice — this is real.

### Step 1: Inventory (10 min)

Fill in this table for your team. Be honest — mark things as missing or outdated if they are.

```
┌────────────────────────┬──────────┬──────────────────┬────────────┐
│ Category               │ Status   │ Location         │ Quality    │
│                        │          │                  │ (1-5)      │
├────────────────────────┼──────────┼──────────────────┼────────────┤
│ Coding guidelines      │ ✅/⚠️/❌ │                  │            │
│ Style guide            │ ✅/⚠️/❌ │                  │            │
│ Architecture docs      │ ✅/⚠️/❌ │                  │            │
│ Design patterns used   │ ✅/⚠️/❌ │                  │            │
│ Tech stack & versions  │ ✅/⚠️/❌ │                  │            │
│ Repository structure   │ ✅/⚠️/❌ │                  │            │
│ PR/code review process │ ✅/⚠️/❌ │                  │            │
│ Deployment pipeline    │ ✅/⚠️/❌ │                  │            │
│ Branching strategy     │ ✅/⚠️/❌ │                  │            │
│ Testing standards      │ ✅/⚠️/❌ │                  │            │
│ Test naming/structure  │ ✅/⚠️/❌ │                  │            │
│ API documentation      │ ✅/⚠️/❌ │                  │            │
│ Error handling patterns│ ✅/⚠️/❌ │                  │            │
│ Onboarding guide       │ ✅/⚠️/❌ │                  │            │
│ Domain/business logic  │ ✅/⚠️/❌ │                  │            │
│ Proprietary tech docs  │ ✅/⚠️/❌ │                  │            │
└────────────────────────┴──────────┴──────────────────┴────────────┘

✅ = Exists and current    ⚠️ = Exists but outdated    ❌ = Missing
```

### Step 2: Gap Analysis (10 min)

Look at your inventory and answer these questions:

**What does Claude need to know that isn't written down?**
Write down 3 things your team does that exist only as tribal knowledge:

```
1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
```

**What exists but isn't AI-consumable?**
Documentation might exist in Confluence or a wiki but be too long, too unstructured, or too scattered for Claude to use effectively. List anything that needs reformatting:

```
1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
```

**What would give your team the biggest immediate impact if Claude knew it?**
Think about the question you'd most want Claude to just *know* when working on your codebase:

```
1. _______________________________________________
```

### Step 3: Action Plan (5 min)

Based on your inventory and gap analysis, create a prioritised list:

**Quick wins** — documentation exists, just needs gathering and formatting:

```
1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
```

**Medium effort** — documentation exists but needs updating:

```
1. _______________________________________________
2. _______________________________________________
```

**Heavy lift** — tribal knowledge that needs to be captured from scratch:

```
1. _______________________________________________
2. _______________________________________________
```

> 💡 **Tip:** Start with the quick wins. Getting 3-4 documents formatted and into your team marketplace in the first week creates momentum. The heavy lifts can happen over weeks 2-4.

---

## Deliverable

You now have a personal **Context Engineering Action Plan** for your team. This feeds directly into your 30-day plan in the Closing session.

Keep this document — you'll reference it in your first biweekly review session with the AI Champion Forum.

---

## Key Takeaways

1. **Context engineering is the "why" behind everything you built today.** Skills, agents, commands, and plugins are only as good as the team knowledge that powers them.

2. **Separate context storage from context application.** Documentation lives in the `context/` folder. Skills and agents *reference* it — they don't duplicate it.

3. **Start with what you have.** Most teams have more documentation than they think — it just needs gathering and reformatting, not writing from scratch.

4. **The four phases are a cycle, not a sequence.** You'll keep discovering new context as you build components and get feedback from your team.

---

## ➡️ Next: Module 7 — Champion as Publisher

Now that you have a plan for *what* to build, the next module defines *how* you'll share it — your ongoing role as an AI Champion.

👉 Open [`07_champion_as_publisher.md`](./07_champion_as_publisher.md)