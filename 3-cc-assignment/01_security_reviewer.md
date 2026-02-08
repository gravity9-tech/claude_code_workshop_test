# Assignment 01: Security Reviewer Agent

**Estimated Duration: 20-30 minutes**

## Objective

Create a `security-reviewer` agent that checks implementations for OWASP Top 10 vulnerabilities, then update `/orchestrate` to include it as a fourth phase.

---

## Background

Your current `/orchestrate` command runs three phases:

```
planner → tdd-guide → code-reviewer
```

After completing this assignment, it will run four phases:

```
planner → tdd-guide → code-reviewer → security-reviewer
```

The security reviewer adds a dedicated security analysis step that checks for common vulnerabilities before the workflow completes.

---

## Requirements

Create a `security-reviewer` agent that:

- Accepts implementation details as input
- Checks for common security vulnerabilities (hint: OWASP Top 10)
- Produces a structured security report with severity levels
- Gives an overall verdict
- Is read-only (no code modifications)

Then update `/orchestrate` to include it as a fourth phase.

---

## Next Assignment

Continue to: [02_full_lifecycle.md](./02_full_lifecycle.md) — Bridging Jira and Development Workflows
