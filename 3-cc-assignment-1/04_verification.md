# Part 4: Verification & Testing

**Estimated Time:** 10 minutes

## Objective

Test your complete workflow and verify that context isolation is working correctly.

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

**Step 2: Stage some changes**

Make a small change to test with:

```bash
# Create a test file
echo "export const TEST_VALUE = 42;" > test-temp.ts
git add test-temp.ts
```

**Step 3: Run the command**

```
/smart-commit
```

Watch the workflow execute:
1. It should detect staged changes
2. The commit-writer agent should generate a message
3. You should see the suggested message
4. Choose to cancel (don't actually commit the test file)

**Step 4: Check context after**

```
/context
```

Compare to your baseline. The context should have grown only by the agent's **result**, not all of its internal work (the git diffs it read, its reasoning, etc.).

**Step 5: Clean up**

```bash
git reset HEAD test-temp.ts
rm test-temp.ts
```

---

## Test 2: Full Commit Flow

Now test with a real change you want to commit.

**Step 1: Make a real change**

Edit a file in your project (documentation, code, config — anything meaningful).

```bash
git add <your-changed-file>
```

**Step 2: Run smart-commit**

```
/smart-commit
```

**Step 3: Review the message**

- Does it follow Conventional Commits format?
- Is the type correct (feat, fix, docs, etc.)?
- Is the description clear and in imperative mood?
- Is the scope appropriate (if detected)?

**Step 4: Confirm or edit**

If the message looks good, confirm to create the commit.

**Step 5: Verify the commit**

```bash
git log -1
```

Check that the commit was created with your approved message.

---

## Test 3: Edge Cases

### No staged changes

```
/smart-commit
```

With nothing staged, the command should inform you that there are no changes to commit.

### Multiple file types

Stage changes across different areas:

```bash
git add src/component.tsx docs/README.md
```

```
/smart-commit
```

Does the agent correctly identify when changes span multiple scopes?

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
