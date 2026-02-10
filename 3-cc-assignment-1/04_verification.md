# Part 4: Verification & Testing

**Estimated Time:** 10 minutes

## Objective

Test your complete workflow using real changes to the tea-store-demo codebase and verify that context isolation is working correctly.

---

## Test 1: Context Isolation

This test verifies that the agent runs in an isolated context.

**Step 1: Clear and check baseline**

```
/clear
```

```
/context
```

Note the context size (should be minimal).

**Step 2: Make a documentation change**

Update the health endpoint's docstring in the backend:

```bash
# Edit backend/app/api/routes.py
# Find the health endpoint and improve its docstring
```

Or use Claude Code:

```
Add a more detailed docstring to the health endpoint in backend/app/api/routes.py
explaining what it returns and when to use it
```

Then stage the change:

```bash
git add backend/app/api/routes.py
```

**Step 3: Run the command**

```
/smart-commit
```

Watch the workflow execute:
1. It should detect staged changes
2. The commit-writer agent should generate a message
3. You should see a suggested message like: `docs(api): improve health endpoint docstring`
4. Choose to cancel for now (we'll do a real commit in Test 2)

**Step 4: Check context after**

```
/context
```

Compare to your baseline. The context should have grown only by the agent's **result**, not all of its internal work (the git diffs it read, its reasoning, etc.).

**Step 5: Reset for the next test**

```bash
git checkout backend/app/api/routes.py
```

---

## Test 2: Feature Change (Frontend)

Test with a real frontend change.

**Step 1: Add a small feature**

Add a hover effect to the ProductCard component:

```bash
# Edit frontend/src/components/ProductCard.tsx
# Add a subtle scale transform on hover
```

Or use Claude Code:

```
Add a subtle hover scale effect (scale 1.02) to the ProductCard component
in frontend/src/components/ProductCard.tsx using Tailwind classes
```

**Step 2: Stage and commit**

```bash
git add frontend/src/components/ProductCard.tsx
```

```
/smart-commit
```

**Step 3: Review the message**

The agent should generate something like:
```
feat(ui): add hover scale effect to ProductCard

Adds subtle visual feedback when users hover over product cards.
```

Verify:
- Type is `feat` (new feature) or `style` (visual change)
- Scope detected as `ui` or `components`
- Description is in imperative mood

**Step 4: Confirm the commit**

If the message looks good, confirm to create the commit.

**Step 5: Verify**

```bash
git log -1
```

---

## Test 3: Backend Change

Test with a backend API change.

**Step 1: Add a field to the health endpoint**

```bash
# Edit backend/app/api/routes.py
# Add a "version" field to the health endpoint response
```

Or use Claude Code:

```
Add a "version" field with value "1.0.0" to the health endpoint
response in backend/app/api/routes.py
```

**Step 2: Stage and run**

```bash
git add backend/app/api/routes.py
/smart-commit
```

**Step 3: Review**

Expected message format:
```
feat(api): add version field to health endpoint
```

---

## Test 4: Multi-Scope Changes

Test how the agent handles changes across multiple areas.

**Step 1: Make changes in multiple locations**

```bash
# Make a small change in both frontend and backend
# For example: update a comment in each
```

**Step 2: Stage both**

```bash
git add frontend/src/services/api.ts backend/app/api/routes.py
```

**Step 3: Run smart-commit**

```
/smart-commit
```

**Step 4: Observe scope handling**

When changes span multiple scopes, the agent should either:
- Omit the scope: `chore: update API documentation`
- Use a general scope: `docs(api): update endpoint documentation`

---

## Test 5: Edge Case - No Changes

```bash
git reset HEAD .  # Unstage everything
```

```
/smart-commit
```

The command should inform you that there are no staged changes to commit.

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
- [ ] Command asks for user confirmation
- [ ] Command executes `git commit` when confirmed

### Context Isolation
- [ ] `/context` before and after shows isolation working
- [ ] Agent's internal work doesn't pollute main context

### End-to-End
- [ ] Successfully created a real commit using `/smart-commit`

---

## Congratulations!

You've built a complete **skill → agent → command** workflow that demonstrates:

1. **Skills** as reusable knowledge modules
2. **Agents** running in isolated contexts with skill injection
3. **Commands** orchestrating agents via the Task tool
4. **Context isolation** keeping your main session clean

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
- `src/api/*` → scope: `api`
- `src/components/*` → scope: `ui`
- `tests/*` → scope: `test`

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
