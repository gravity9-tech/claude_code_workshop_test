# Module 6: Champion as Publisher

⏱️ **Duration:** 20 minutes  
📋 **Format:** Discussion + planning exercise  

---

## What You'll Learn

By the end of this module you'll have:
- A clear picture of your ongoing role as an AI Champion post-bootcamp
- A quality checklist for marketplace contributions
- An understanding of the publish-review-iterate cycle
- A concrete 30-day action plan for your team

---

## 1. Your Three Hats (7 min)

As an AI Champion, you wear three hats. Today's bootcamp has prepared you for all of them:

### 🔨 Builder — Create Components for Your Team

This is what you did in Modules 1 and 2. You know how to create skills, agents, and commands that encode your team's knowledge and workflows.

**Post-bootcamp, this means:**
- Identifying repetitive tasks your team does manually
- Building skills that encode your team's coding standards, testing conventions, and architectural patterns
- Creating agents for focused work like implementation, review, or documentation
- Packaging these into plugins that any team member can install and use

**The question to keep asking:** *"What does our team do repeatedly that Claude could do better with the right context?"*

### 🔍 Curator — Select and Adapt from the Marketplace

Not everything needs to be built from scratch. The Pandora marketplace will grow as more teams contribute.

**Post-bootcamp, this means:**
- Browsing the organisation-level marketplace for plugins that fit your team
- Adapting shared plugins to your team's specific tech stack (e.g., taking the `tdd-workflow` skill and adjusting it for your team's test framework)
- Evaluating plugin quality before recommending to your team
- Flagging gaps — if your team needs something that doesn't exist, request it through the Forum

**The question to keep asking:** *"Has another team already solved this?"*

### 📤 Publisher — Share Your Team's Innovations

This is where you create value beyond your own team. When you build something reusable, you contribute it back so other teams benefit.

**Post-bootcamp, this means:**
- Identifying which of your team's plugins could work across teams
- Generalising team-specific components (removing hardcoded paths, project names, framework-specific assumptions)
- Writing documentation that developers outside your team can follow
- Submitting PRs to the Pandora marketplace repo

**The question to keep asking:** *"Would this be useful to other teams if I made it generic?"*

---

## 2. The Publish-Review-Iterate Cycle (7 min)

### Spotting Reusable Patterns

The best marketplace contributions come from real daily work, not hypothetical exercises. Watch for these signals:

- **You built a skill** and a teammate says *"I wish I had that last week"* → candidate for team marketplace
- **Two teams build similar components** independently → candidate for organisation marketplace
- **You keep copy-pasting the same prompt** across projects → that's a skill or command waiting to happen
- **A new team member struggles** with something your plugin handles → proof it's worth sharing

### Quality Checklist

Before contributing a plugin to the marketplace, run through this checklist:

```
Plugin Contribution Checklist:
- [ ] Works in at least one real project (tested, not theoretical)
- [ ] README explains what it does and when to use it
- [ ] manifest.json has accurate name, description, roles, and tags
- [ ] Skill descriptions include 3+ example trigger phrases
- [ ] Agent contracts define clear input/output expectations
- [ ] No hardcoded paths, project names, or team-specific assumptions
- [ ] Follows conventional commit messages in any scripts
- [ ] Versioned (start at 1.0.0, follow semver)
```

### The Review Process

```
You build it → PR to marketplace repo → Peer review by another champion → Merge → Available to all teams
```

**What reviewers check:**
- Does it work when installed in a different project?
- Is the documentation clear enough for someone outside the author's team?
- Does it follow the plugin structure (manifest.json, proper directories)?
- Are there any security concerns (hardcoded credentials, unsafe bash commands)?

**Versioning rules:**
- **Patch** (1.0.1): Bug fixes, documentation updates
- **Minor** (1.1.0): New features, additional skills within a plugin
- **Major** (2.0.0): Breaking changes to input/output contracts

---

## 3. AI Champion Forum Integration (6 min)

The AI Champion Forum is where the community comes together. Here's how your bootcamp work connects to it:

### Biweekly Reviews

Every two weeks, champions meet to share progress, surface challenges, and demo new work.

**What to bring:**
- New plugins or components you've built
- Adoption numbers (how many on your team are using Claude Code regularly?)
- Challenges or gaps you've encountered
- Requests for plugins from other teams

### Friday Demos

The Forum feeds into wider engineering visibility through regular demo sessions.

**Good demo candidates from your team:**
- A workflow that saved significant time (e.g., `/implement-feature` turning a 2-hour task into 20 minutes)
- A creative use of agents or Ralph Loop that others haven't seen
- A before/after comparison showing Claude Code with vs. without context

### Cross-Team Sharing

The marketplace enables a multiplier effect:

```
Team A builds a code-review plugin for React
    → Publishes to Pandora marketplace
        → Team B adapts it for Angular
            → Publishes the adapted version
                → Team C uses it for Vue with minimal changes
```

Each contribution makes the next team's job easier. That's how 27 teams accelerate each other instead of reinventing the wheel.

---

## 4. Your 30-Day Action Plan (included in Closing session)

The Closing session after this module will give you time to write your plan in detail. To prepare, start thinking about:

### Week 1–2: Context Discovery
- Use the context map from Module 5 to inventory your team's documentation
- Identify the top 3 gaps (knowledge that exists in people's heads but not in docs)
- Schedule a kickoff with your team to explain what's coming

### Week 2–3: Marketplace Setup
- Clone the marketplace template for your team
- Add your team's context documentation
- Install relevant plugins from the Pandora marketplace

### Week 3–4: First Plugin
- Build your first team-specific plugin (start small — one skill is enough)
- Test it with at least two team members
- If it's reusable, submit a PR to the Pandora marketplace

### Ongoing
- Present at the next AI Champion Forum
- Participate in biweekly review sessions
- Track adoption on your team (who's using Claude Code, what for, how often)

---

## The Cadence

Here's what your ongoing rhythm looks like:

| Frequency | Activity |
|-----------|----------|
| **Daily** | Use Claude Code in your own work — lead by example |
| **Weekly** | Check in with team members: what's working, what's not |
| **Biweekly** | Attend AI Champion Forum review session |
| **Monthly** | Contribute at least one component to the team or Pandora marketplace |
| **Quarterly** | Review and update your team's context documentation |

---

## Key Takeaway

You're not just a Claude Code user — you're a **context engineer** for your team. The components you build, curate, and publish determine how effective Claude Code is for every developer on your team. The marketplace turns individual effort into organisational capability.

> *"The best AI Champion isn't the one who uses Claude Code the most. It's the one whose team uses it the most — because the context and plugins are so good that everyone benefits."*

---

## ➡️ Next: Closing — Team Action Plans

Time to write your concrete 30-day plan and commit to your first deliverable.

👉 Open [`07_closing.md`](./07_closing.md)