# Assignment 1: Git Commit Workflow

**Prerequisite:** Complete Workshop 1

**Estimated Time:** 30-45 minutes

## Overview

In this assignment, you'll build a complete **skill → agent → command** workflow that automates meaningful git commits. This tests your understanding of all core Workshop 1 concepts.

## What You'll Build

```
.claude/
├── agents/
│   └── commit-writer.md        # Agent that generates commit messages
├── commands/
│   └── smart-commit.md         # Command that orchestrates the workflow
└── skills/
    └── commit-standards/
        └── SKILL.md            # Skill teaching commit message standards
```

## Learning Outcomes

| Concept | How It's Tested |
|---------|-----------------|
| Skills | Create `commit-standards` with clear guidelines |
| Skill Injection | Agent injects the skill via frontmatter |
| Agents | `commit-writer` generates commit messages |
| Commands | `/smart-commit` orchestrates via Task tool |

## Assignment Parts

| Part | Topic | File |
|------|-------|------|
| 1 | [Create the Commit Standards Skill](./01_commit_standards_skill.md) | `commit-standards/SKILL.md` |
| 2 | [Create the Commit Writer Agent](./02_commit_writer_agent.md) | `commit-writer.md` |
| 3 | [Create the Smart Commit Command](./03_smart_commit_command.md) | `smart-commit.md` |
| 4 | [Verification & Testing](./04_verification.md) | — |

## The Workflow

```
User: /smart-commit
         │
         ▼
┌─────────────────┐
│  Check git      │  Step 1: Check for staged/unstaged changes
│  state          │
└────────┬────────┘
         │
         ▼ (if unstaged only)
┌─────────────────┐
│  Stage changes  │  Step 1b: Show changes, ask to stage all/select
│  (if needed)    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  commit-writer  │  Step 2: Agent analyzes diff and generates message
│  agent          │          (injects commit-standards skill)
│  (isolated)     │
└────────┬────────┘
         │ returns commit message
         ▼
┌─────────────────┐
│  Present to     │  Step 3: Show message and ask for confirmation
│  user           │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Execute commit │  Step 4: Run git commit with the message
└─────────────────┘
```

## Success Criteria

Your assignment is complete when:

- [ ] The `commit-standards` skill exists and defines clear commit message guidelines
- [ ] The `commit-writer` agent injects the skill and generates messages based on staged changes
- [ ] The `/smart-commit` command orchestrates the full workflow
- [ ] A real commit can be made using your workflow

Start with [01_commit_standards_skill.md](./01_commit_standards_skill.md)
