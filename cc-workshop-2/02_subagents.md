# Workshop 02: Mastering Subagents

**Duration: ~16 minutes**

## What You'll Learn

- What subagents are and when to use them
- Built-in agents (Explore, Plan)
- Create agents that use skills
- Run agents in parallel

---

## What Are Subagents?

Subagents are autonomous Claude instances that:
- Work independently on tasks
- Can run in parallel
- Have specific tools and focus
- Report back results

---

## Built-in Agents

| Agent | Purpose |
|-------|---------|
| `Explore` | Find files, understand architecture |
| `Plan` | Design implementation approaches |

---

**Locations:**
- Project: `.claude/agents/`
- Personal: `~/.claude/agents/`

---

## Task 1: Using Built-in Agents

Try these prompts on the workshop codebase:

```
Explore the FastAPI application structure and explain how routes connect to mock_data
```

```
Find all files related to product filtering and price validation
```

```
Plan how to add a wishlist endpoint to app/api/routes.py
```

---

## Task 2: Create the Agents Directory

```bash
mkdir -p .claude/agents
```

---

## Task 3: Create a Code Reviewing Agent

This agent uses the `reviewing-code` skill we created earlier. The `skills` field in frontmatter tells the agent which skills to load.

Create `.claude/agents/reviewing-code.md`:

```markdown
---
description: Reviews code for security vulnerabilities, bugs, and quality issues. Use for code reviews, security audits, and PR reviews.
skills: reviewing-code
allowed-tools: Read, Grep, Glob
---

# Code Review Agent

You are a code review agent that performs thorough security and quality reviews.

## Process

1. **Identify scope** - Determine which files to review
2. **Security review** - Check for vulnerabilities using the security checklist
3. **Quality review** - Check code structure using the quality checklist
4. **Generate report** - Output findings in the standard format

## Behavior

- Always read files before reviewing
- Check for the most critical security issues first
- Provide specific line numbers and fix suggestions
- Use severity levels: Critical, Warning, Info

## Output

Generate a structured review report with:
- Summary table (files reviewed, issue counts)
- Issues grouped by severity
- Specific fixes for each issue
```

**Key points:**
- `skills: reviewing-code` — Loads the skill's reference files on demand
- `description` — Contains trigger words that match the skill's description
- Agent provides the workflow; skill provides the checklists

---

## Task 4: Running Agents in Parallel (Within Claude)

Run multiple agents simultaneously in a single prompt:

```
Run these tasks in parallel:
1. Review app/api/routes.py for security vulnerabilities
2. Review app/models.py for missing field validations
```

Another example - exploration and review:

```
Run in parallel:
1. Explore the codebase structure and list all Python files
2. Review app/mock_data.py for any hardcoded values that should be configurable
```

Claude spawns separate agents that work simultaneously and report back.

---

## Task 5: Running Agents in Separate Terminals

For long-running or independent tasks, use multiple terminal windows:

**Terminal 1 - Security Review:**
```bash
claude -p "Review app/api/routes.py for security vulnerabilities"
```

**Terminal 2 - Quality Review:**
```bash
claude -p "Review app/models.py for code quality issues"
```

**Terminal 3 - Documentation:**
```bash
claude -p "Generate docs for this codebase using component mermaid format"
```

| Approach | Best For |
|----------|----------|
| `Run in parallel...` (within Claude) | Quick coordinated tasks, shared context |
| Separate terminals with `-p` | Long tasks, different focus areas |
---

## Your Structure

```
.claude/
├── commands/
│   └── test-basket.md
├── skills/
│   └── reviewing-code/
│       ├── SKILL.md
│       └── reference/
│           └── ...
└── agents/
    └── reviewing-code.md      # Uses the reviewing-code skill
```

---

## Checkpoint

- [ ] Used the Explore agent
- [ ] Used the Plan agent
- [ ] Created the `reviewing-code.md` agent with skill reference
- [ ] Understand how agents use skills via frontmatter
- [ ] Ran agents in parallel within Claude
- [ ] Tried running Claude in separate terminals

---

## Workshop Complete!

You've learned:
1. `/init` to generate CLAUDE.md
2. Custom slash commands with arguments
3. Multi-file skills with reference folders
4. Agents that use skills
5. Running agents in parallel

### Final Structure

```
.claude/
├── commands/
│   └── test-basket.md
├── skills/
│   └── reviewing-code/
│       ├── SKILL.md
│       └── reference/
│           ├── security-checklist.md
│           ├── quality-checklist.md
│           └── output-format.md
└── agents/
    └── reviewing-code.md
```

---

## Next Up

Continue to: [03_hooks.md](./03_hooks.md) - Automation hooks
