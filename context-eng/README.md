# Context Engineering

**Stop the Token Drain & Drift. Engineer your context.**

## The Problem

```
  Prompt ──────────────────────────────► Large Codebase
  "Fix the login bug"                   ┌──────────────────┐
                                        │ 500+ files       │
                        ┌──────────┐    │ src/             │
                        │ AI Agent │───►│ lib/             │──► Reads EVERYTHING
                        └──────────┘    │ utils/           │    $$$ Token Drain
                                        │ tests/           │    😵 Drift
                                        │ ...              │
                                        └──────────────────┘
```

## The Solution — Context Engineering

```
                        ┌─────────────────────────────────────────────┐
                        │           Context Engineering Stack         │
  Prompt                │                                             │
  "Fix the login bug"   │  ┌───────────────┐    ┌──────────────────┐ │
         │              │  │ Navigation    │    │ Memory           │ │
         │              │  │               │    │                  │ │
         ▼              │  │ Codemaps      │    │ CLAUDE.md        │ │
  ┌──────────┐          │  │ DeepWiki      │───►│ Rules/           │ │
  │ AI Agent │─────────►│  │ Architecture  │    │ Auto-memory      │ │
  └──────────┘          │  │ Index         │    │ Skills           │ │
                        │  └───────┬───────┘    └────────┬─────────┘ │
                        │          │                     │           │
                        │          ▼                     ▼           │
                        │  ┌─────────────────────────────────────┐   │
                        │  │ Reads only 3 relevant files         │   │
                        │  │ Follows YOUR patterns               │   │
                        │  │ Concise, accurate output            │   │
                        │  └─────────────────────────────────────┘   │
                        └─────────────────────────────────────────────┘
```

## What This Is

A guide for developers adopting AI tools on real codebases. Context Engineering is the practice of structuring what AI sees, knows, and remembers — so it works with your codebase, not against your token budget.

## The Docs

### [Part 1: Token Drain & Drift](./01-token-drain-and-drift/)
The problem. What developers experience, why it happens, and what it costs.

### [Part 2: Why Prompt Engineering Isn't Enough](./02-why-prompt-engineering-isnt-enough/)
Prompt engineering is the last mile. You need every mile built.

### [Part 3: Context Engineering](./03-context-engineering/)
The solution. The 3R Framework, the Context Stack, and the core principles.

### [Part 4: Implementation Guide](./04-implementation-guide/)
Hands-on setup. CLAUDE.md, rules, auto-memory, navigation layers, output discipline, and skills.

### [Part 5: Adoption Playbook](./05-adoption-playbook/)
From quick wins to team rollout. Measuring results and avoiding anti-patterns.

## Start Here

If you have 15 minutes: [Quick Wins](./05-adoption-playbook/01-quick-wins.md)
If you want the full picture: [Part 1](./01-token-drain-and-drift/01-the-real-experience.md)
