# Workshop 08: Skills

**Duration: ~15 minutes**

## What You'll Learn

- What skills are and how they differ from MCP tools
- How to create a custom skill with `SKILL.md`
- Using `$ARGUMENTS` for dynamic input
- Building a `/fix-issue` skill that integrates with Jira

---

## Tools vs Skills

In the previous module, you connected Jira MCP which gave Claude **tools** like `jira_search` and `jira_create_issue`. Tools are single operations.

**Skills** are markdown files that teach Claude *how* to use tools effectively for a specific workflow.

| | MCP Tools | Skills |
|---|-----------|--------|
| **What they are** | API connections | Markdown instructions |
| **What they provide** | Capabilities (actions) | Knowledge (how-to) |
| **Example** | `jira_get_issue` | `/fix-issue` |
| **Scope** | Single operation | Multi-step workflow |

**Key insight:** MCP gives Claude the **ability** to read Jira issues. A skill teaches Claude the **process** for fixing one.

---

## Skill Structure

Every skill is a folder with a `SKILL.md` file:

```
.claude/skills/
└── my-skill/
    └── SKILL.md
```

`SKILL.md` has two parts:

```yaml
---
name: my-skill
description: What this does. Use when...
---

# Instructions

Steps Claude follows when this skill is active.
```

- **Frontmatter** (between `---`) — metadata Claude uses to decide when to load it
- **Body** — instructions Claude follows

---

## Where Skills Live

| Location | Path | Scope |
|----------|------|-------|
| Personal | `~/.claude/skills/<name>/SKILL.md` | All your projects |
| Project | `.claude/skills/<name>/SKILL.md` | Shared with team via git |

---

## Task 1: Create a Simple Skill

Create a skill that standardizes how Claude writes commit messages:

```bash
mkdir -p .claude/skills/commit-style
```

Create `.claude/skills/commit-style/SKILL.md`:

```yaml
---
name: commit-style
description: Commit message standards. Use when creating git commits.
---

When writing commit messages, follow these rules:

1. Use conventional commit format: type(scope): description
2. Types: feat, fix, docs, style, refactor, test, chore
3. Keep the subject line under 72 characters
4. Use imperative mood ("add feature" not "added feature")

Examples:
- feat(auth): add OAuth2 login flow
- fix(api): handle null response from user endpoint
- docs(readme): update installation instructions
```

Now `/clear` and ask Claude to commit something. It should follow your commit style automatically.

---

## Task 2: Build the `/fix-issue` Skill

This is the skill that ties everything together. It reads a Jira ticket via MCP and guides Claude through fixing it.

```bash
mkdir -p .claude/skills/fix-issue
```

Create `.claude/skills/fix-issue/SKILL.md`:

```yaml
---
name: fix-issue
description: Fix a Jira issue end-to-end
disable-model-invocation: true
argument-hint: <JIRA-TICKET-ID>
---

# Fix Issue Workflow

Fix the Jira issue: $ARGUMENTS

## Steps

1. **Read the ticket**: Use the Atlassian MCP to fetch the issue details for $ARGUMENTS. Understand the description, acceptance criteria, and any comments.

2. **Explore the codebase**: Identify which files are relevant to the issue. Read them to understand the current behavior.

3. **Plan the fix**: Before writing code, explain what needs to change and why.

4. **Implement**: Make the code changes. Follow the project's coding conventions.

5. **Test**: Run existing tests to make sure nothing is broken. Write new tests if the fix introduces new behavior.

6. **Commit**: Create a commit following the project's commit conventions. Reference the ticket ID in the commit message.

7. **Summary**: Report what was changed, which files were modified, and confirm tests pass.
```

Note the key pieces:
- **`disable-model-invocation: true`** — only you can trigger this (prevents Claude from auto-running it)
- **`argument-hint`** — shows `<JIRA-TICKET-ID>` in the autocomplete menu
- **`$ARGUMENTS`** — gets replaced with whatever you type after `/fix-issue`

---

## Task 3: Test `/fix-issue`

Create a test ticket in your Jira project first (or use an existing one). Then:

```
/fix-issue PROJECT-123
```

Replace `PROJECT-123` with your actual ticket ID. Watch Claude:

1. Fetch the ticket from Jira via MCP
2. Read relevant code files
3. Explain its plan
4. Implement the fix
5. Run tests
6. Commit with the ticket reference

---

## Skill Invocation Modes

| Frontmatter | Who can invoke | Use for |
|-------------|---------------|---------|
| (default) | You + Claude | Reference knowledge (conventions, patterns) |
| `disable-model-invocation: true` | Only you | Workflows with side effects (commits, deploys) |
| `user-invocable: false` | Only Claude | Background knowledge Claude applies automatically |

Use `disable-model-invocation: true` for anything that **does something** — commits, PRs, deployments. You don't want Claude deciding to deploy because the code looks ready.

---

## Key Takeaways

- **Skills** teach Claude workflows; **MCP tools** provide capabilities
- `$ARGUMENTS` makes skills dynamic — pass ticket IDs, file names, etc.
- Use **`disable-model-invocation: true`** for skills with side effects
- Skills + MCP together = powerful automation (e.g., `/fix-issue` + Jira MCP)

---

Continue to: [09_full_workflow.md](./09_full_workflow.md)
