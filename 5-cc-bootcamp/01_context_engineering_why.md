# Opening: Context Engineering — The Why

⏱️ **Duration:** 20 minutes  
📋 **Format:** Presentation + Group Discussion  

---

## What You'll Learn

By the end of this session you'll understand:
- What Context Engineering is and why it matters more than prompt engineering
- The four-phase process we use to make Claude Code team-aware
- How the three-tier marketplace organises shared knowledge
- Your role as an AI Champion in this system
- What we're building today and how the three repos connect

---

## 1. The Problem: Claude Without Context (8 min)

### Group Discussion

Think about when you use Claude Code on your team's codebase. What's the first thing it gets wrong?

Common answers across teams:
- "It doesn't know our coding standards"
- "It suggests patterns we don't use"
- "It doesn't understand our architecture"
- "It creates branches with the wrong naming convention"
- "It writes tests in a framework we don't use"

Every one of these is the same problem: **Claude doesn't have your team's context.**

### Prompt Engineering vs. Context Engineering

Claude Code out of the box is a generic developer. It's brilliant — but it's never seen your codebase, your conventions, or your workflows. It's like hiring a senior contractor who doesn't know your codebase yet.

**Prompt engineering** tries to solve this by writing better questions. It helps, but it puts the burden on *every developer, every time*. If someone on your team writes a vague prompt, they get generic output — regardless of how good the tool is.

**Context engineering** solves it differently. Instead of teaching every developer to write better prompts, you **teach Claude your team's way of working once** — and every developer benefits automatically.

| | Prompt Engineering | Context Engineering |
|---|---|---|
| **Who does the work?** | Every developer, every prompt | AI Champion, once (then maintained) |
| **What improves?** | Individual prompt quality | Claude's baseline understanding of your team |
| **How it scales** | Linearly — better prompts, one at a time | Multiplicatively — one skill benefits all developers |
| **Output** | Better responses to specific questions | Claude that follows your standards by default |

### The Simple Definition

> **Context Engineering** is the process of capturing your team's knowledge — coding guidelines, architecture patterns, workflows, testing standards — and making it available to Claude Code through skills, agents, commands, and marketplace plugins.

The difference in practice:

- **Without context:** *"Claude, write me a service"* → generic code in whatever style Claude defaults to
- **With context:** *"Claude, write me a service"* → code that follows YOUR patterns, YOUR conventions, YOUR testing standards — automatically, because the skills and agents carry that knowledge

---

## 2. The Four Phases (5 min)

Context Engineering follows a structured four-phase process. You don't need to memorise these — you'll experience them during today's bootcamp and then apply them to your own team over the coming weeks.

### Phase 1: Context Discovery
What does your team already know? We collect:
- Coding guidelines and style guides
- Architectural documentation and patterns
- Tech stack inventory and dependencies
- Repository structures and conventions
- Common workflows and processes
- Testing standards and practices

If it's documented, we gather and standardise it. If it's tribal knowledge that lives only in people's heads, we help capture it.

### Phase 2: Marketplace Setup
Once we have the context, we structure it into reusable components:
- Clone the team marketplace template
- Integrate team-specific context
- Add shared components from the Pandora marketplace
- Organise by role (architect, frontend, backend, QA)

👉 *This is what we'll learn in **Module 3** today.*

### Phase 3: Implementation Loop
Build the actual skills, agents, and commands that make Claude useful for daily work:
- Identify priority components to build
- Implement with context engineering support
- Test and validate with real workflows
- Iterate based on team feedback

👉 *This is what we'll do in **Modules 1 and 2** today.*

### Phase 4: Continuous Evolution
Context isn't static. Teams evolve, standards change, new people join:
- Launch components to the team
- Run biweekly review sessions
- Update and improve based on usage data
- Share wins across teams via the AI Champion Forum

👉 *This is what **Module 7** (Champion as Publisher) prepares you for.*

---

## 3. Pandora's Context Challenge (5 min)

### Why This Matters at Scale

We have 27 teams across diverse tech stacks — JDK 8 and 21, Angular, Node, React, Apex, Lightning, .NET Framework, X++, and more. A skill that works perfectly for a React team is meaningless for a .NET team. A branching convention for one team might contradict another's.

If we tried to give every developer a single set of prompts, it would work for nobody. Context engineering lets us customise Claude for each team while sharing common patterns across the organisation.

### The Three-Tier Marketplace

This is how we organise context at scale:

```
┌─────────────────────────────────────────┐
│          Organisation Level             │
│  Shared across all business lines       │
│  and teams                              │
│  (Jira integration, Git conventions,    │
│   TDD workflow, common patterns)        │
├─────────────────────────────────────────┤
│         Business Line Level             │
│  Shared across teams within a           │
│  business line                          │
│  (React patterns, Node API standards,   │
│   .NET conventions, Apex standards)     │
├─────────────────────────────────────────┤
│             Team Level                  │
│  Specific to an individual team's       │
│  conventions and domain                 │
│  (Checkout API patterns, CRM           │
│   integration, billing conventions)     │
└─────────────────────────────────────────┘
```

- **Organisation level:** Components that work across all business lines and teams — the feature lifecycle toolkit we'll build today falls here
- **Business line level:** Plugins shared across all teams within a business line — tech stack conventions, framework patterns, and API design standards
- **Team level:** Your team's specific context, domain patterns, and service conventions

### Your Role as AI Champion

As an AI Champion, you sit at the intersection of all three tiers:

| Hat | What you do | Example |
|-----|-------------|---------|
| **Builder** | Create components for your team | Build a skill for your team's API design standards |
| **Curator** | Select and adapt plugins from the marketplace | Take the org-level TDD workflow and adapt it for your team's test framework |
| **Publisher** | Share your team's innovations back to the organisation | Contribute a useful agent that other teams can adopt |

Today's bootcamp equips you for all three.

---

## 4. Today's Mission (2 min)

### What We're Building

Every module today is a practical application of context engineering:

| Module | What you'll do | Context Engineering Phase |
|--------|---------------|--------------------------|
| **Module 2:** Key Components | See how MCP, skills, agents, and commands layer together in a feature implementation lifecycle | Understanding the building blocks |
| **Module 3:** Marketplace Intro | Understanding the role of a marketplace to share context and best practices across teams | Phase 3: Implementation |
| **Module 4:** Plugins & Marketplace Architecture | Package everything into a plugin, publish it, install it in another project | Phase 2: Marketplace Setup |
| **Module 5:** Ralph Loop & LISA | Run the feature lifecycle autonomously | Advanced automation |
| **Module 6:** Context Engineering, How | Map your own team's context gaps and build an action plan | Phase 1: Discovery |
| **Module 7:** Champion as Publisher | Define your ongoing role maintaining your team's marketplace | Phase 4: Evolution |

### Three Repos

We're using three repositories today:

| Repo | What it is | How we'll use it |
|------|-----------|-----------------|
| `claude_code_workshop` | Tea Shop sample project (React + FastAPI) | Our sandbox — install plugins here and test them |
| `claude_code_workshop_test` | Workshop instructions (this repo) | The `cc-bootcamp/` folder has today's step-by-step guides |
| `claude_code_marketplace_demo` | Marketplace template | Build and publish plugins here |

You already know the first two from the workshops. The marketplace repo is new — that's where the plugin packaging and publishing happens.

### ✅ Setup Check

Before we move on, confirm you have:

- [ ] Claude Code installed and working
- [ ] Tea Shop repo (`claude_code_workshop`) cloned and runnable
- [ ] This instructions repo (`claude_code_workshop_test`) cloned
- [ ] Marketplace demo repo (`claude_code_marketplace_demo`) cloned
- [ ] Jira MCP configured (from Workshop 1)

> ⚠️ If anything isn't set up, flag it now — we'll sort it during the first exercise.

---

## Reference Card: Context Engineering at a Glance

Keep this handy throughout the day:

```
┌──────────────────────────────────────────────────┐
│         CONTEXT ENGINEERING AT A GLANCE           │
├──────────────────────────────────────────────────┤
│                                                   │
│  WHAT: Capturing team knowledge and making it     │
│  available to Claude Code through reusable        │
│  components (skills, agents, commands, plugins)   │
│                                                   │
│  WHY: Claude without context = generic developer  │
│       Claude with context = team member who       │
│       knows your standards, patterns & workflows  │
│                                                   │
│  THE FOUR PHASES:                                 │
│  1. Discovery   → Collect & standardise docs      │
│  2. Marketplace → Structure into components       │
│  3. Implement   → Build skills, agents, commands  │
│  4. Evolve      → Biweekly reviews & updates      │
│                                                   │
│  THE THREE TIERS:                                 │
│  • Organisation  → Shared across all bus. lines   │
│  • Business Line → Shared across teams in a BL    │
│  • Team          → Specific to one team           │
│                                                   │
│  YOUR ROLE AS AI CHAMPION:                        │
│  Builder  → Create components for your team       │
│  Curator  → Select & adapt from the marketplace   │
│  Publisher → Share your team's innovations         │
│                                                   │
└──────────────────────────────────────────────────┘
```

---

## ➡️ Next: Module 1 — Key Components

Now that you understand *why* we're building these components, let's see *how* they work together. Module 2 walks through a complete feature implementation lifecycle — from Jira ticket to reviewed code — using different component types.

👉 Open [`02_key_components.md`](./02_key_components.md)