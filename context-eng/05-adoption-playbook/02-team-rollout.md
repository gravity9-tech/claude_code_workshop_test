# Team Rollout

Context Engineering scales from one developer to a full team. Here's how to roll it out.

## Step 1: Version Control the Basics

Commit these to your repo:

```
CLAUDE.md                    # Team-shared conventions
.claude/
├── rules/                   # Scoped rules
│   ├── code-style.md
│   └── testing.md
└── settings.json            # Shared project settings
```

Add to `.gitignore`:
```
.claude/settings.local.json  # Personal overrides
```

## Step 2: Agree on Conventions

CLAUDE.md is a team document. Treat it like you'd treat a `CONTRIBUTING.md`:

- Review it in PRs when conventions change
- Keep it authoritative — if it says "use pattern X", the team uses pattern X
- One owner or rotating maintainer prevents contradictions

## Step 3: Scope Rules by Team/Domain

In monorepos, rules map to ownership:

```
.claude/rules/
├── frontend/react.md        # paths: src/components/**
├── backend/api.md           # paths: src/api/**
├── infra/deployment.md      # paths: infra/**
```

Each team maintains their rules. No one needs to load everything.

## Step 4: Personal Layer

Each developer can add personal preferences:

- `~/.claude/CLAUDE.md` — personal style preferences
- `~/.claude/rules/` — personal rule files
- `.claude/settings.local.json` — local overrides (not committed)

These layer on top of the team setup without conflicting.

## Step 5: Evolve

- Review CLAUDE.md quarterly — remove stale rules, add new patterns
- Watch for recurring drift — if AI keeps getting something wrong, add a rule
- Let auto-memory accumulate, then promote stable learnings to CLAUDE.md
