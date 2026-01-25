# Workshop 02: Creating Custom Subagents

**Duration: ~12 minutes**

## What You'll Learn

- When to create custom subagents vs using built-in ones
- The AGENT.md file structure and frontmatter
- Use the `agent-creator` skill to generate agents
- Create a standalone code-reviewer agent

---

## When to Create Custom Subagents

Built-in agents (Explore, Plan, general-purpose) are general-purpose. Create custom agents when you need:

| Need | Example |
|------|---------|
| **Domain specialization** | Security auditor, accessibility checker |
| **Tool restrictions** | Read-only reviewer, limited bash access |
| **Model requirements** | Opus for complex analysis, Haiku for speed |
| **Workflow encapsulation** | Combine multiple skills into one execution unit |

---

## Agent File Locations

| Scope | Location | Use Case |
|-------|----------|----------|
| Project | `.claude/agents/` | Team-shared, project-specific |
| Personal | `~/.claude/agents/` | Your agents across all projects |

Claude Code automatically detects and loads agents from these directories.

---

## The Agent Creator Skill

Just like we used `skill-creator` for skills and `command-creator` for commands, we have an `agent-creator` skill for generating well-structured agents.

Verify it's available:

```
/skills
```

You should see `agent-creator` in the list.

---

## AGENT.md Structure

The agent-creator will generate files with this structure:

```yaml
---
name: agent-name
description: When Claude should delegate to this agent
tools: Tool1, Tool2, Bash(pattern:*)
model: sonnet
---

# Agent Title

Instructions for what this agent does...
```

### Frontmatter Fields

| Field | Required | Purpose | Example |
|-------|----------|---------|---------|
| `name` | Yes | Agent identifier (@agent-name) | `code-reviewer` |
| `description` | Yes | When to delegate | `Reviews code for security` |
| `tools` | No | Restrict available tools | `Read, Grep, Glob` |
| `model` | No | Override model | `opus`, `sonnet`, `haiku` |

---

## Task: Create a Code Reviewer Agent

Let's use the agent-creator to build a code review agent.

### Step 1: Prompt the agent-creator

```
Create an agent called "code-reviewer" that reviews code for quality, security, and best practices.

It should:
- Be read-only (no Write or Edit tools)
- Have access to git diff for reviewing changes
- Focus on: security vulnerabilities, performance issues, code maintainability
- Output a structured review with severity levels
- Use sonnet model for balanced analysis
```

### Step 2: Review the generated agent

The agent-creator will:
1. Research best practices for code review agents
2. Design appropriate frontmatter (tools, model)
3. Create structured instructions
4. Validate against its checklist
5. Save to `.claude/agents/code-reviewer.md`

### Step 3: Verify agent loads

Restart Claude Code to load the new agent:

```bash
/exit
claude
```

Check available agents:

```
/agents
```

You should see:
- Built-in: Explore, Plan, general-purpose
- Custom: **code-reviewer**

---

## Step 4: Test the Agent

Ask Claude to use the agent:

```
Review the cart.ts file for security issues and code quality
```

**Watch for:**
1. Claude recognizes this is a code review task
2. Claude delegates to the code-reviewer agent
3. Agent analyzes the file using its restricted tools
4. Structured review returns to main conversation

---

## Understanding the Generated Agent

The agent-creator produces agents with:

**Appropriate Tools**:
```yaml
tools: Read, Grep, Glob, Bash(git diff:*)
```
- Read-only access for safety
- Git diff for reviewing changes
- No Write/Edit to prevent modifications

**Clear Trigger Description**:
```yaml
description: Reviews code for quality, security, and best practices.
             Use when auditing code, checking PRs, or reviewing changes.
```

**Structured Output**:
```markdown
## Output Format
### Critical
- [Issues]
### High
- [Issues]
### Summary
- Verdict: Approve / Request Changes
```

---

## Key Principle: Agents Are Execution Units

```
┌─────────────────────────────────────────────────────────────┐
│                                                              │
│   SKILLS = What to know (domain expertise, patterns)        │
│                                                              │
│   AGENTS = How to execute (isolated runtime, specific tools)│
│                                                              │
│   Next module: Combine them by injecting skills into agents │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

The code-reviewer agent has its own instructions. In the next module, we'll create agents that **inject existing skills** for their domain expertise.

---

## Checkpoint

You've now:

- [ ] Understand when to create custom vs use built-in agents
- [ ] Know the AGENT.md structure and frontmatter fields
- [ ] Used `agent-creator` to generate code-reviewer agent
- [ ] Verified the agent loads with `/agents`
- [ ] Tested the agent with a review task

---

## Next Up

Continue to [03_skills_and_agents.md](./03_skills_and_agents.md) to inject your Workshop 1 skills into custom agents.
