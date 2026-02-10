# Part 2: Create the Commit Writer Agent

**Estimated Time:** 10 minutes

## Objective

Create an agent that analyzes staged git changes and generates a well-formatted commit message. The agent will inject the `commit-standards` skill you created in Part 1.

## Background

An agent runs in an **isolated context window**. This means:
- It gets a fresh context when it starts
- It can read files, run commands, and reason independently
- Only its final result returns to the caller
- Its internal work doesn't pollute the main context

By injecting the `commit-standards` skill, the agent gains knowledge about commit message formatting without you having to repeat those instructions.

## Your Task

Use the `/agent-creator` to create an agent called `commit-writer`.

The agent should:

### Input
- No explicit input needed — it will analyze the current git state

### Process
1. Run `git diff --staged` to see what changes are staged
2. Run `git diff --staged --stat` to get a summary of files changed
3. Analyze the changes to understand:
   - What type of change is this? (feat, fix, refactor, etc.)
   - What scope/area of the codebase is affected?
   - What is the purpose of these changes?
4. Generate a commit message following the injected `commit-standards` skill

### Output
Return a structured response:
```
## Suggested Commit Message

<the generated commit message>

## Analysis

- **Type**: <detected type>
- **Scope**: <detected scope or "none">
- **Files changed**: <count>
- **Summary**: <brief explanation of what the changes do>
```

---

## Instructions

In Claude Code, type:

```
/agent-creator Create an agent called "commit-writer"

The agent should:
- Inject the "commit-standards" skill for commit message knowledge
- Analyze staged git changes to generate a commit message
- Run git diff --staged to see the actual changes
- Run git diff --staged --stat to see files changed
- Determine the appropriate commit type (feat, fix, docs, etc.)
- Detect the scope from the file paths if possible
- Generate a commit message following the injected skill's standards
- Be read-only for code (only needs Bash for git commands, Read for context)

Output format:
- The suggested commit message in a code block
- Analysis section explaining the detected type, scope, and summary

Tools needed: Bash (for git commands), Read, Glob, Grep (for code context)
```

---

## Verify Your Work

Check that the agent was created:

```bash
cat .claude/agents/commit-writer.md
```

Verify the frontmatter includes:
- [ ] `name: commit-writer`
- [ ] `skills:` section that includes `commit-standards`
- [ ] `tools:` that includes at least `Bash` for git commands

---

## Test the Agent

Make a real change to the tea-store-demo codebase and test the agent.

**Step 1: Make a change**

Edit a file — for example, add a comment to the cart context:

```bash
# Edit frontend/src/contexts/CartContext.tsx
# Add a comment explaining the localStorage key used for persistence
```

**Step 2: View unstaged changes**

Before staging, see what you changed:

```bash
git diff                  # See the actual code changes
git diff --stat           # See summary of files changed
```

This shows your working directory changes that aren't staged yet.

**Step 3: Stage the changes**

```bash
git add frontend/src/contexts/CartContext.tsx
```

**Step 4: View staged changes**

Now verify what's staged for commit:

```bash
git diff --staged         # See the actual staged changes
git diff --staged --stat  # See summary of staged files
```

This is what the agent will analyze.

**Step 5: Run the agent**

In Claude Code:

```
Use the commit-writer agent to generate a commit message for my staged changes
```

The agent should:
1. Run `git diff --staged` to analyze the changes
2. Detect the type (likely `docs` or `chore` for comments)
3. Detect the scope from the file path (`cart`)
4. Return a properly formatted commit message

**Step 6: Reset your test change**

```bash
git reset HEAD frontend/src/contexts/CartContext.tsx   # Unstage
git checkout frontend/src/contexts/CartContext.tsx     # Discard changes
```

---

## Next

Continue to [03_smart_commit_command.md](./03_smart_commit_command.md)
