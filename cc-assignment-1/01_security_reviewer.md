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

---

## Part 1: Create the Agent

Create the `security-reviewer` agent based on the requirements above.

Verify it exists at `.claude/agents/security-reviewer.md`

---

## Part 2: Test in Isolation

Before integrating, test the agent directly on some existing code in your project. Verify it produces a structured security report with severity levels and specific findings.

---

## Part 3: Update `/orchestrate`

Modify `.claude/commands/orchestrate.md` to:

1. Add security review as a fourth phase after code-reviewer
2. Pass the implementation summary and plan to the security reviewer
3. Include security verdict in the final report
4. Flag the workflow if CRITICAL ISSUES are found

---

## Part 4: Test End-to-End

Run the updated `/orchestrate` with a feature that involves user input (forms, API endpoints, etc.) and verify:

- The security review phase runs after code review
- Security findings appear in the final report
- The overall verdict reflects security status

---

## Success Criteria

- [ ] `security-reviewer` agent exists in `.claude/agents/`
- [ ] Agent produces structured security reports with severity levels
- [ ] `/orchestrate` now runs 4 phases
- [ ] Final report includes security verdict and findings summary
- [ ] Tested end-to-end with a feature that handles user input

---

## Next Assignment

Continue to: [02_full_lifecycle.md](./02_full_lifecycle.md) — Bridging Jira and Development Workflows
