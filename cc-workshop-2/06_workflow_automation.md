# Workshop 06: Workflow Automation

**Duration: ~10 minutes**

## What You'll Learn

- Turn slash commands into multi-step workflows
- Use arguments to make workflows reusable
- Chain tasks for consistent processes

---

## What is a Workflow?

A **workflow** is a slash command that automates multiple steps:

| Single Command | vs | Workflow Command |
|----------------|-----|------------------|
| "Review this file" | | Review + test + fix + commit |
| One-shot task | | Multi-step process |
| Manual each time | | Consistent every time |

---

## Basic Workflow Structure

```markdown
# .claude/commands/my-workflow.md

Do these steps in order:

1. First step
2. Second step
3. Third step
```

Invoke with `/project:my-workflow` and Claude executes all steps.

---

## Task 1: Create a Review Workflow

This workflow uses our existing `reviewing-code` skill:

Create `.claude/commands/review-and-fix.md`:

```markdown
---
description: Review code, then fix any issues found
arguments:
  - name: target
    description: File or folder to review
    required: true
---

Review and fix workflow for: $ARGUMENTS

1. Review the target using the reviewing-code skill
2. List all critical and warning issues found
3. Fix each issue one by one
4. Run tests to verify fixes don't break anything
5. Summarize what was fixed
```

**Test it:**
```
/project:review-and-fix src/auth.py
```

---

## Task 2: Create a Feature Workflow

Create `.claude/commands/add-feature.md`:

```markdown
---
description: Scaffold a new feature with tests
arguments:
  - name: feature
    description: Feature name (e.g., user-profile)
    required: true
---

Add new feature: $ARGUMENTS

1. Create the feature file in `src/features/`
2. Add basic structure with error handling
3. Create a test file in `tests/`
4. Add at least 3 test cases
5. Run tests to verify everything passes
```

**Test it:**
```
/project:add-feature shopping-cart
```

---

## Task 3: Create a PR Prep Workflow

Create `.claude/commands/prep-pr.md`:

```markdown
---
description: Prepare changes for a pull request
arguments:
  - name: description
    description: Brief description of changes
    required: true
---

Prepare PR: $ARGUMENTS

1. Run all tests and fix any failures
2. Review changed files for code quality
3. Update any affected documentation
4. Create a descriptive commit message
5. Show a summary ready for PR description
```

**Test it:**
```
/project:prep-pr "Add user authentication"
```

---

## Workflow Patterns

### Sequential Steps

```markdown
1. Do this first
2. Then do this
3. Finally do this
```

### Conditional Steps

```markdown
1. Run tests
2. If tests fail, fix the issues
3. If tests pass, proceed to commit
```

### Using Skills in Workflows

```markdown
1. Review code using the reviewing-code skill
2. Apply fixes based on the review output
```

---

## Your Structure

```
.claude/
├── commands/
│   ├── review-and-fix.md    # Review workflow
│   ├── add-feature.md       # Feature workflow
│   └── prep-pr.md           # PR prep workflow
├── skills/
│   └── reviewing-code/      # Used by workflows
└── agents/
```

---

## Checkpoint

- [ ] Created a multi-step workflow command
- [ ] Used `$ARGUMENTS` for reusable workflows
- [ ] Tested a workflow with a real target
- [ ] Understand how workflows chain with skills

---

## Workshop Complete!

You've learned:
1. Skills for domain expertise
2. Subagents that use skills
3. Hooks for automation
4. Marketplace for discovery
5. Creating and sharing plugins
6. **Workflow commands for multi-step processes**

Go build something great!
