# Workshop 04: Building Reusable Skills

**Duration: ~12 minutes**

## What You'll Learn

- Difference between commands and skills
- Correct skill file format (SKILL.md)
- Multi-file skills with `reference/` folder
- Progressive disclosure pattern (load files on demand)
- Naming conventions (gerund form: `reviewing-code`)

---

## Commands vs Skills

| | Commands | Skills |
|---|----------|--------|
| **Purpose** | Do a specific task | Provide expertise/knowledge |
| **Location** | `.claude/commands/file.md` | `.claude/skills/name/SKILL.md` |
| **Invocation** | `/command-name` | Auto-invoked or referenced |
| **Structure** | Single file | Directory with SKILL.md |

---

## Skill File Format

Skills live in directories with a required `SKILL.md` file:

```
.claude/skills/
└── my-skill/
    └── SKILL.md
```

### SKILL.md Format

```markdown
---
name: my-skill-name
description: What this skill does. When to use it.
allowed-tools: Read, Grep, Glob
---

# Skill Title

Your expertise and instructions here.
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Lowercase, hyphens only (e.g., `reviewing-code`) |
| `description` | Yes | What it does + when to use it (max 1024 chars) |
| `allowed-tools` | No | Tools Claude can use with this skill |

**Name rules:** lowercase letters, numbers, hyphens only. No spaces or underscores.

### Naming Conventions

Use **gerund form** (verb + -ing) for skill names:

| Good (Gerund) | Acceptable | Avoid |
|---------------|------------|-------|
| `reviewing-code` | `code-review` | `code-reviewer` |
| `processing-pdfs` | `pdf-processor` | `pdf-helper` |
| `analyzing-data` | `data-analysis` | `utils` |
| `writing-docs` | `doc-writer` | `helper` |

### Multi-File Structure

For complex skills, use a `reference/` folder:

```
skill-name/
├── SKILL.md              # Overview (always loaded first)
└── reference/
    ├── topic-one.md      # Loaded when needed
    ├── topic-two.md      # Loaded when needed
    └── examples.md       # Loaded when needed
```

**Why?** Progressive disclosure—Claude only loads files when relevant, saving context tokens.

---

## Task 1: Create the Skills Directory

```bash
mkdir -p .claude/skills/reviewing-code/reference
```

---

## Task 2: Create a Code Reviewing Skill (Multi-File)

This skill demonstrates the recommended multi-file structure with a `reference/` folder for progressive disclosure—Claude only loads files when needed.

### Directory Structure

```
.claude/skills/reviewing-code/
├── SKILL.md                    # Main overview (always loaded)
└── reference/
    ├── security-checklist.md   # Loaded when security review needed
    ├── quality-checklist.md    # Loaded when quality review needed
    └── output-format.md        # Loaded when generating report
```

### Create the Main SKILL.md

Create `.claude/skills/reviewing-code/SKILL.md`:

```markdown
---
name: reviewing-code
description: Reviews code for security vulnerabilities, quality issues, and bugs. Triggers on code review requests, security audits, PR reviews, or quality checks. Analyzes code structure, identifies issues, and provides actionable fixes.
allowed-tools: Read, Grep, Glob
---

# Code Review

Senior code reviewer applying security and quality best practices.

## Quick Start

1. Identify files to review
2. Determine review type (security, quality, or both)
3. Apply relevant checklist
4. Generate structured report

## Review Types

**Security Review**: See [reference/security-checklist.md](reference/security-checklist.md)
- Injection vulnerabilities
- Authentication issues
- Data exposure risks

**Quality Review**: See [reference/quality-checklist.md](reference/quality-checklist.md)
- Code structure
- Maintainability
- Error handling

**Output Format**: See [reference/output-format.md](reference/output-format.md)
```

### Create Reference Files

Create `.claude/skills/reviewing-code/reference/security-checklist.md`:

```markdown
# Security Checklist

## Injection Vulnerabilities

### SQL Injection
- [ ] Parameterized queries used
- [ ] No string concatenation in queries
- [ ] ORM methods used correctly

```python
# Bad
query = f"SELECT * FROM users WHERE id = {user_id}"

# Good
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

### Command Injection
- [ ] No shell=True with user input
- [ ] Arguments passed as lists
- [ ] Input sanitized before exec

### XSS (Cross-Site Scripting)
- [ ] Output encoded/escaped
- [ ] CSP headers configured
- [ ] No dangerouslySetInnerHTML with user data

## Authentication & Authorization

- [ ] Passwords hashed (bcrypt/argon2)
- [ ] No secrets in code or logs
- [ ] Session tokens are secure
- [ ] Authorization checked on each request

## Data Exposure

- [ ] Sensitive data not logged
- [ ] API responses filtered
- [ ] Error messages don't leak internals
```

Create `.claude/skills/reviewing-code/reference/quality-checklist.md`:

```markdown
# Quality Checklist

## Code Structure

### Function Design
- [ ] Single responsibility
- [ ] Under 30 lines preferred
- [ ] Clear input/output types
- [ ] No side effects when possible

### Naming
- [ ] Descriptive variable names
- [ ] Consistent conventions
- [ ] No abbreviations (except common ones)

## Error Handling

- [ ] Errors caught at boundaries
- [ ] Specific exception types
- [ ] Meaningful error messages
- [ ] No silent failures

```python
# Bad
try:
    process()
except:
    pass

# Good
try:
    process()
except ValidationError as e:
    logger.warning(f"Validation failed: {e}")
    raise UserError("Invalid input") from e
```

## Maintainability

- [ ] No magic numbers (use constants)
- [ ] Complex logic commented
- [ ] Dead code removed
- [ ] Dependencies up to date
```

Create `.claude/skills/reviewing-code/reference/output-format.md`:

```markdown
# Review Output Format

## Structure

Generate review reports using this format:

```markdown
## Review Summary

| Metric | Count |
|--------|-------|
| Files reviewed | X |
| Critical issues | X |
| Warnings | X |
| Suggestions | X |

## Issues

### [file]:[line] - Critical
**Issue**: [Description of the problem]
**Impact**: [What could go wrong]
**Fix**: [Specific code suggestion]

### [file]:[line] - Warning
**Issue**: [Description]
**Fix**: [Suggestion]

### [file]:[line] - Info
**Suggestion**: [Improvement idea]
```

## Severity Levels

| Level | Description |
|-------|-------------|
| **Critical** | Security vulnerabilities, data loss risks, crashes |
| **Warning** | Bugs, poor patterns, missing validation |
| **Info** | Style improvements, minor optimizations |

## Example Output

```markdown
## Review Summary

| Metric | Count |
|--------|-------|
| Files reviewed | 3 |
| Critical issues | 1 |
| Warnings | 2 |
| Suggestions | 1 |

## Issues

### src/auth.py:45 - Critical
**Issue**: SQL injection vulnerability in user lookup
**Impact**: Attacker can extract or modify database
**Fix**: Use parameterized query
\`\`\`python
cursor.execute("SELECT * FROM users WHERE email = ?", (email,))
\`\`\`

### src/utils.py:12 - Warning
**Issue**: Bare except clause hides errors
**Fix**: Catch specific exceptions
```
```

---

## Task 3: Using Skills

Skills are auto-invoked based on your task description. Just describe what you need:

```
Review this code for security vulnerabilities and quality issues
```

```
Check this file for potential bugs and code quality problems
```

```
Audit the authentication module for security issues
```

Claude matches your task to the right skill based on the description field. No need to name the skill explicitly!

---

## Your Structure

```
.claude/
├── commands/
│   └── test-basket.md
└── skills/
    └── reviewing-code/
        ├── SKILL.md
        └── reference/
            ├── security-checklist.md
            ├── quality-checklist.md
            └── output-format.md
```

---

## Checkpoint

- [ ] Created `.claude/skills/reviewing-code/reference/` structure
- [ ] Created SKILL.md with proper frontmatter and references
- [ ] Created reference files (security, quality, output format)
- [ ] Understand progressive disclosure (files loaded on demand)
- [ ] Tested the skill with a code review request

---

## Next Up

Continue to: [02_subagents.md](./02_subagents.md) - Agents with skills
