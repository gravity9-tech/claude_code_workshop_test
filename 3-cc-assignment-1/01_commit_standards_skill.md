# Part 1: Create the Commit Standards Skill

**Estimated Time:** 10 minutes

## Objective

Create a skill that teaches Claude how to write excellent commit messages following the Conventional Commits standard.

## Background

A skill is a knowledge file that teaches Claude *how* to do something. In this case, you're encoding your team's commit message standards so that any agent can inject this knowledge.

## Your Task

Use the `/skill-creator` to create a skill called `commit-standards`.

The skill should teach:

### 1. Conventional Commits Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### 2. Commit Types

| Type | When to Use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding or updating tests |
| `chore` | Maintenance tasks, dependencies |

### 3. Guidelines

- **Description**: Imperative mood ("add feature" not "added feature")
- **Length**: Subject line under 72 characters
- **Scope**: Optional, indicates what part of codebase (e.g., `api`, `ui`, `auth`)
- **Body**: Explain *what* and *why*, not *how*
- **Breaking changes**: Use `!` after type or `BREAKING CHANGE:` in footer

### 4. Examples

Good:
```
feat(cart): add quantity selector to cart items

Users can now adjust item quantities directly from the cart page
instead of removing and re-adding items.
```

```
fix(auth): prevent session timeout during active use

Closes #234
```

Bad:
```
fixed stuff          # Vague, no type
Updated the code     # No type, past tense, vague
FEAT: Add feature    # Wrong format (caps, colon placement)
```

---

## Instructions

In Claude Code, type:

```
/skill-creator Create a skill called "commit-standards"

The skill should teach how to write excellent git commit messages
following the Conventional Commits standard.

Include:
1. The commit message format: <type>(<scope>): <description>
2. All commit types: feat, fix, docs, style, refactor, test, chore
3. Guidelines for writing good descriptions (imperative mood, under 72 chars)
4. How to handle scope (optional, indicates area of codebase)
5. When and how to write commit bodies (explain what and why)
6. How to indicate breaking changes (! or BREAKING CHANGE footer)
7. Good and bad examples

The skill should be comprehensive enough that an agent injecting it
can generate high-quality commit messages for any code change.
```

---

## Verify Your Work

Check that the skill was created:

```bash
cat .claude/skills/commit-standards/SKILL.md
```

The skill should include:
- [ ] Clear format specification
- [ ] All commit types with descriptions
- [ ] Writing guidelines
- [ ] Examples of good and bad commits

---

## Next

Continue to [02_commit_writer_agent.md](./02_commit_writer_agent.md)
