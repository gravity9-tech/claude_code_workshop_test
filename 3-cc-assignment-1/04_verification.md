# Part 4: Verification & Testing

**Estimated Time:** 10 minutes

## Objective

Test your complete workflow using real changes to the tea-store-demo codebase.

---

## Test 1: Commit Your Work

You already have unstaged work from building the skill, agent, and command. Let's use `/smart-commit` to commit it.

**Step 1: View your changes**

```bash
git status                # See changed files
git diff --stat           # See summary
```

You should see your new files in `.claude/`.

**Step 2: Run smart-commit**

```
/smart-commit
```

**Step 3: Observe the auto-stage prompt**

The command should:
1. Detect no staged changes
2. Detect your unstaged changes
3. Show `git diff --stat` output
4. Ask if you want to stage all, select files, or cancel

**Step 4: Stage and review**

Choose **yes** to stage all changes, then review the generated message.

Verify:
- Type is appropriate (likely `feat` or `chore`)
- Scope makes sense
- Description is clear and in imperative mood

**Step 5: Confirm the commit**

If the message looks good, confirm to create the commit.

**Step 6: Verify the commit**

```bash
git log -1 --oneline
```

You should see your commit with the generated message.

---

## Test 2: Frontend Change

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

## Test 3: Edge Case - No Changes

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

## Test 4: Pre-Staged Changes (Optional)

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

## Congratulations!

You've built a complete **skill → agent → command** workflow that demonstrates:

1. **Skills** as reusable knowledge modules
2. **Agents** running with skill injection
3. **Commands** orchestrating agents via the Task tool

This pattern scales to complex workflows — just add more skills, agents, and commands as needed.

---

## What's Next?

Continue to **Workshop 2** to learn about:
- Multi-agent architectures (planner → implementer → reviewer)
- TDD workflows
- Advanced orchestration patterns
