# Part 4: Verification & Testing

**Estimated Time:** 10 minutes

## Objective

Test your complete workflow using real changes to the tea-store-demo codebase.

---

## Test 1: Auto-Stage Flow (Dry Run)

First, test the staging functionality without actually committing.

**Step 1: Make a backend change**

Open `backend/app/api/routes.py` in your editor.

Find this line near the top (around line 8):

```python
from app.models import Product
```

Add a new line right after it:

```python
VALID_CATEGORIES = ["black", "green", "oolong", "herbal"]
```

Save the file.

**Step 2: View your changes**

```bash
git diff                  # See the actual changes
git diff --stat           # See summary
```

**Step 3: Run smart-commit (without staging first)**

```
/smart-commit
```

**Step 4: Observe the auto-stage prompt**

The command should:
1. Detect no staged changes
2. Detect your unstaged changes in `routes.py`
3. Show `git diff --stat` output
4. Ask if you want to stage all, select files, or cancel

**Step 5: Cancel this time**

Choose **cancel** — we'll commit this change in the next test.

---

## Test 2: Complete Commit Flow

Now let's commit the backend change for real.

**Step 1: Run smart-commit again**

```
/smart-commit
```

**Step 2: Stage when prompted**

Choose **yes** to stage all changes.

**Step 3: Review the generated message**

The agent should generate something like:

```
feat(api): add VALID_CATEGORIES constant

Defines valid tea categories as a reusable constant.
```

Verify:
- Type is `feat` or `refactor`
- Scope is `api` or `backend`
- Description is clear and in imperative mood

**Step 4: Confirm the commit**

If the message looks good, confirm to create the commit.

**Step 5: Verify the commit**

```bash
git log -1 --oneline
```

You should see your commit with the generated message.

---

## Test 3: Frontend Change

Add a visual enhancement to the ProductCard component.

**Step 1: Make a frontend change**

Open `frontend/src/components/shared/ProductCard.tsx` in your editor.

Find this line (around line 27):

```tsx
<div data-testid="product-card" className="product-card bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300">
```

Replace it with (adds hover scale effect):

```tsx
<div data-testid="product-card" className="product-card bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl hover:scale-[1.02] transition-all duration-300">
```

Save the file.

**Step 2: Run smart-commit**

```
/smart-commit
```

The command will detect unstaged changes and offer to stage them.

**Step 3: Stage and review**

Choose to stage, then review the generated message. Expected format:

```
style(ui): add hover scale effect to ProductCard
```

**Step 4: Confirm and verify**

Confirm the commit, then verify:

```bash
git log -2 --oneline
```

You should now see both commits.

---

## Test 4: Edge Case - No Changes

Verify the command handles the "nothing to commit" case gracefully.

**Step 1: Check working directory**

```bash
git status
```

Should show "nothing to commit, working tree clean" (since we committed everything).

**Step 2: Run smart-commit**

```
/smart-commit
```

**Step 3: Observe the response**

The command should inform you that there are no changes to commit and exit gracefully.

---

## Test 5: Pre-Staged Changes (Optional)

Test that the command works when changes are already staged.

**Step 1: Make and stage a change**

Open `backend/app/api/routes.py` and find this line (around line 10):

```python
router = APIRouter()
```

Add a comment above it:

```python
# API router for tea product endpoints
router = APIRouter()
```

Save and stage manually:

```bash
git add backend/app/api/routes.py
```

**Step 2: Run smart-commit**

```
/smart-commit
```

**Step 3: Observe the flow**

This time the command should:
1. Detect staged changes exist
2. Skip the staging prompt
3. Go directly to generating the commit message

**Step 4: Complete or cancel**

Confirm to create a third commit, or cancel if you prefer.

---

## Checklist

Your assignment is complete when all items are checked:

### Files Created
- [ ] `.claude/skills/commit-standards/SKILL.md` exists
- [ ] `.claude/agents/commit-writer.md` exists
- [ ] `.claude/commands/smart-commit.md` exists

### Skill Verification
- [ ] Skill defines Conventional Commits format
- [ ] Skill includes all commit types (feat, fix, docs, etc.)
- [ ] Skill has examples of good and bad commits

### Agent Verification
- [ ] Agent frontmatter includes `skills: commit-standards`
- [ ] Agent analyzes `git diff --staged`
- [ ] Agent outputs properly formatted commit messages

### Command Verification
- [ ] Command uses Task tool to spawn agent
- [ ] Command checks for staged changes first
- [ ] Command offers to stage unstaged changes if none are staged
- [ ] Command asks for user confirmation before committing
- [ ] Command executes `git commit` when confirmed

### End-to-End
- [ ] Successfully created at least one real commit using `/smart-commit`
- [ ] Verified commit with `git log`

---

## Congratulations!

You've built a complete **skill → agent → command** workflow that demonstrates:

1. **Skills** as reusable knowledge modules
2. **Agents** running with skill injection
3. **Commands** orchestrating agents via the Task tool

This pattern scales to complex workflows — just add more skills, agents, and commands as needed.

---

## Bonus Challenges

Ready for more? Try these extensions:

### 1. Pre-commit Validation Agent

Create a `pre-commit-checker` agent that:
- Runs linting on staged files
- Runs relevant tests
- Returns pass/fail status

Update `/smart-commit` to run this agent before `commit-writer`.

### 2. Commit Scope Detection

Enhance `commit-writer` to auto-detect scope from file paths:
- `backend/*` → scope: `api`
- `frontend/src/components/*` → scope: `ui`
- `frontend/src/services/*` → scope: `services`

### 3. Breaking Change Detection

Add logic to detect potential breaking changes:
- Removed exports
- Changed function signatures
- Modified public APIs

Warn the user and suggest using `!` in the commit type.

---

## What's Next?

Continue to **Workshop 2** to learn about:
- Multi-agent architectures (planner → implementer → reviewer)
- TDD workflows
- Advanced orchestration patterns
