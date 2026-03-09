# Devin AI Engineering Playbook
### Best Practices for Effective Implementation

---

## 1. Devin is an Autonomous Agent, Not a Code Completer

The first and most critical mindset shift is understanding what Devin actually is. Unlike traditional copilots that offer passive, line-by-line code suggestions, Devin is a fully autonomous engineering agent that operates end-to-end across your development workflow.

```
┌─────────────────────────────────┐     ┌─────────────────────────────────────┐
│     Traditional Copilots        │     │           Devin AI                  │
│                                 │     │                                     │
│  ○ Code completion              │     │  → Executes commands                │
│  ○ Syntax suggestions           │     │  → Navigates repositories           │
│  ○ Passive assistance           │     │  → Runs build pipelines             │
│                                 │     │  → Iterates autonomously            │
│   ┌──────┐    ┌──────┐          │     │  → Verifies fixes before commit     │
│   │ IDE  │───▶│ Hint │          │     │                                     │
│   └──────┘    └──────┘          │     │   ┌──────┐  ┌───────┐  ┌────────┐   │
│                                 │     │   │ Code │─▶│ Build │─▶│ Test   │   │
│   You drive, AI suggests.       │     │   └──────┘  └───────┘  └────────┘   │
│                                 │     │       │                     │       │
│                                 │     │       └────── Fix ◀─────────┘       │
│                                 │     │                                     │
│                                 │     │   You delegate, Devin delivers.     │
└─────────────────────────────────┘     └─────────────────────────────────────┘
```

Because of this autonomous capability, **clear instructions and task scoping are critical for effective results**. The quality of what Devin produces is directly proportional to the clarity and structure of what you give it.

---

## 2. Treat Devin Like a Newly Hired Engineer

Do not prompt Devin like a chatbot. It doesn't need casual conversation — it needs engineering context. Think of Devin as a competent but new team member who has never seen your codebase before. They can write good code, but they need you to point them in the right direction.

```
  ┌─────────────────────────┐          ┌──────────────────────────────────────┐
  │         ✗  BAD          │          │            ✓  GOOD                   │
  │                         │          │                                      │
  │                         │          │  Investigate the failing unit test   │
  │    "Fix this bug."      │          │  in `checkoutHelpers.spec.js`.       │
  │                         │          │  The failure occurs in               │
  │                         │          │  `setPliPromotion`.                  │
  │                         │          │  Identify the root cause and         │
  │                         │          │  implement a fix without changing    │
  │                         │          │  business logic.                     │
  └─────────────────────────┘          └──────────────────────────────────────┘
```

**Always provide:** context about the environment, repository structure, constraints on what should and shouldn't change, and clear acceptance criteria. Without these, Devin will make assumptions — and assumptions lead to wasted sessions and rework.

---

## 3. The Anatomy of an Effective Devin Prompt

Every task you assign to Devin should clearly define both the objective and its bounds. A well-structured prompt contains five key elements:

```
  ┌──────────────────────────────────────────────────────────────────┐
  │                                                                  │
  │  ┌──────────────────────┐                                        │
  │  │  🗺  CONTEXT         │  The environment and repository        │
  │  │                      │  structure Devin will be working in.   │
  │  └──────────────────────┘                                        │
  │            │                                                     │
  │            ▼                                                     │
  │  ┌──────────────────────┐                                        │
  │  │  🎯  TASK            │  The clear objective and goal.         │
  │  │                      │  What exactly needs to be done.        │
  │  └──────────────────────┘                                        │
  │            │                                                     │
  │            ▼                                                     │
  │  ┌──────────────────────┐                                        │
  │  │  🔒  CONSTRAINTS     │  What the agent is explicitly          │
  │  │                      │  forbidden from doing or changing.     │
  │  └──────────────────────┘                                        │
  │            │                                                     │
  │            ▼                                                     │
  │  ┌──────────────────────┐                                        │
  │  │  ✅  ACCEPTANCE      │  The definitive measure of success     │
  │  │     CRITERIA         │  (definition of done).                 │
  │  └──────────────────────┘                                        │
  │            │                                                     │
  │            ▼                                                     │
  │  ┌──────────────────────┐                                        │
  │  │  📦  OUTPUT          │  What specific files, PRs, and         │
  │  │     REQUIREMENTS     │  explanations must be returned.        │
  │  └──────────────────────┘                                        │
  │                                                                  │
  └──────────────────────────────────────────────────────────────────┘
```

### Example: A High-Quality Engineering Prompt

```
┌──────────────────────────────────────────────────────────────────────┐
│  prompt.txt / request.md                                             │
│                                                                      │
│  This repository contains a Salesforce Commerce Cloud      [Context] │
│  integration layer.                                                  │
│                                                                      │
│  Fix failing unit tests in OMSHelper.spec.js related to:    [Task]   │
│  getFormattedTotal, updateRecentOrderModel,                          │
│  updateOrderModelsWithResponse.                                      │
│                                                                      │
│  Do not change business logic.                          [Constraints]│
│                                                                      │
│  All unit tests pass.                           [Acceptance Criteria]│
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Notice how concise yet complete this is. Every line serves a purpose: Devin knows where it's working, what to fix, what not to touch, and how to know when it's done.

---

## 4. Provide Explicit Repository Context Before Execution

Before asking Devin to implement changes, establish **exactly where it should work**. This is especially important for monorepos or projects with multiple sub-applications. If multiple repositories or directories exist (e.g., SFRA and PWA in a commerce platform), clearly specify the app structure and where specifically Devin needs to look for context to prevent hallucinated paths.

```
  commerce-platform/
  │
  ├── SFRA/                    ◀── Legacy storefront (do NOT touch)
  │   ├── controllers/
  │   ├── models/
  │   └── templates/
  │
  └── PWA/                     ◀── Target application (work HERE)
      ├── components/          ◀── React components
      ├── hooks/               ◀── Custom React hooks
      └── services/            ◀── API integration layer
```

Without this explicit direction, Devin may search the wrong directories, reference outdated patterns from the legacy app, or create files in unexpected locations. A few seconds of context saves hours of rework.

---

## 5. Define Ironclad Constraints to Prevent Scope Creep

Devin may assume the freedom to restructure code unless boundaries are explicitly defined. Clear constraints prevent unintended architecture changes. This is perhaps the most commonly overlooked element of Devin prompts — and the one that causes the most damage when missing.

```
  ┌──────────────────────────┐          ┌───────────────────────────┐
  │   ✗  Do NOT Modify       │          │   ✗  Do NOT Modify        │
  │                          │          │                           │
  │   Existing controller    │          │   API response            │
  │   logic.                 │          │   structures.             │
  └──────────────────────────┘          └───────────────────────────┘
               │                                    │
               │         ┌──────────┐               │
               └────────▶│   🔒     │◀──────────────┘
                         │  LOCKED  │
               ┌────────▶│  SCOPE   │◀──────────────┐
               │         └──────────┘               │
               │                                    │
  ┌──────────────────────────┐          ┌───────────────────────────┐
  │   ✗  Do NOT Modify       │          │   ✓  ONLY Modify          │
  │                          │          │                           │
  │   Database schemas.      │          │   Helper functions.       │
  │                          │          │                           │
  └──────────────────────────┘          └───────────────────────────┘
```

When writing constraints, be as explicit as possible. Rather than saying "don't break anything," specify exactly which files, modules, patterns, APIs, or schemas are off-limits. Positive constraints (what Devin *should* modify) are equally important — they narrow the scope and reduce the chance of unintended side effects.

---

## 6. Break Large Transformations Into Iterative Phases

Large transformations reduce reliability. Incremental tasks significantly improve Devin's success rate. This is one of the most important operational principles: **never ask Devin to boil the ocean.**

```
  ┌──────────────────────────────┐     ┌──────────────────────────────────┐
  │       ✗  BAD APPROACH        │     │       ✓  BETTER APPROACH         │
  │                              │     │                                  │
  │  ┌────────────────────────┐  │     │  ┌───────────────────────────┐   │
  │  │                        │  │     │  │  Phase 3:                 │   │
  │  │                        │  │     │  │  Update frontend          │   │
  │  │    Rewrite the entire  │  │     │  │  integration.             │   │
  │  │    platform using      │  │     │  └───────────┬───────────────┘   │
  │  │    microservices.      │  │     │              │                   │
  │  │                        │  │     │  ┌───────────┴───────────────┐   │
  │  │         ⚠              │  │     │  │  Phase 2:                 │   │
  │  │                        │  │     │  │  Expose API               │   │
  │  └────────────────────────┘  │     │  │  endpoints.               │   │
  │                              │     │  └───────────┬───────────────┘   │
  │  Overwhelming scope.         │     │              │                   │
  │  High failure rate.          │     │  ┌───────────┴───────────────┐   │
  │  Unpredictable output.       │     │  │  Phase 1:                 │   │
  │                              │     │  │  Extract payment logic    │   │
  │                              │     │  │  into a service module.   │   │
  │                              │     │  └───────────────────────────┘   │
  │                              │     │                                  │
  │                              │     │  Each phase = 1 Devin session.   │
  │                              │     │  Each session = reviewable PR.   │
  └──────────────────────────────┘     └──────────────────────────────────┘
```

Each phase should be independently verifiable and produce a clean, mergeable pull request. This approach yields higher success rates, lower ACU consumption, smaller code review surfaces, and the ability to course-correct early if Devin takes a wrong turn.

---

## 7. The Ask Devin Pre-Flight Protocol

Use Ask Devin to analyse an issue *before* starting a session. This improves prompt quality and prevents unnecessary session usage. Think of this as your pre-flight checklist — you wouldn't take off without one, and you shouldn't start a Devin session without one either.

```
  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
  │   Analyse    │    │   Propose    │    │  Construct   │    │   Execute    │
  │   Problem    │───▶│   Options    │───▶│   Prompt     │───▶│   Session    │
  │              │    │              │    │              │    │              │
  │  Ask Devin   │    │  Ask Devin   │    │  Ask Devin   │    │  Start the   │
  │  to review   │    │  for solution│    │  to write    │    │  actual      │
  │  the issue.  │    │  pathways.   │    │  the prompt  │    │  autonomous  │
  │              │    │              │    │  based on    │    │  run.        │
  │              │    │              │    │  chosen      │    │              │
  │              │    │              │    │  option.     │    │              │
  └──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
        │                    │                    │                    │
        ▼                    ▼                    ▼                    ▼
   Understand the      Evaluate trade-     Get a structured     Let Devin
   scope and           offs before         prompt with full     execute with
   impact.             committing.         context.             confidence.
```

### Case Study: Resolving Snyk Vulnerabilities

This protocol works especially well for security and dependency issues:

```
  ┌──────────────────────────────────────────────────────────────────┐
  │  Step 1:  Ask Devin to analyse the Snyk vulnerability            │
  │           (impact, affected dependencies, options).              │
  └────────────────────────────┬─────────────────────────────────────┘
                               ▼
  ┌──────────────────────────────────────────────────────────────────┐
  │  Step 2:  Review Devin's options                                 │
  │           - Option 1: Upgrade dependency                         │
  │           - Option 2: Replace library                            │
  │           - Option 3: Apply mitigation                           │
  └────────────────────────────┬─────────────────────────────────────┘
                               ▼
  ┌──────────────────────────────────────────────────────────────────┐
  │  Step 3:  Ask Devin to construct a session prompt                │
  │           specifically for Option 1.                             │
  └────────────────────────────┬─────────────────────────────────────┘
                               ▼
  ┌──────────────────────────────────────────────────────────────────┐
  │  Step 4:  Start the Devin session using the generated prompt.    │
  └──────────────────────────────────────────────────────────────────┘

  Benefits: Clearer problem understanding, structured execution,
            fewer iterations.
```

---

## 8. The Standardised End-to-End Execution Flow

A repeatable workflow ensures consistent results and limits architectural damage. Follow this six-step process for every Devin task:

```
  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
  │          │   │          │   │          │   │          │   │          │   │          │
  │ Context  │──▶│  Task    │──▶│Constraint│──▶│ Execute  │──▶│ Review   │──▶│ Validate │
  │          │   │          │   │          │   │          │   │          │   │          │
  └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘
       │              │              │              │              │              │
       ▼              ▼              ▼              ▼              ▼              ▼
   Provide        Define a       Add            Run           Inspect        Run tests
   repository     clear          boundaries.    Devin.        the proposed   and security
   structure.     objective.                                  output.        tools.
```

Each step feeds into the next. Skipping steps — especially Context and Constraints — is the primary cause of failed sessions. The workflow is designed to be used consistently across all task types, whether bug fixes, feature work, migrations, or test generation.

---

## 9. Optimal Use Cases vs. Anti-Patterns

Devin performs best with clear engineering tasks, not vague ideation. Knowing what to assign — and what to keep for yourself — is the key to maximising ROI.

```
  ┌──────────────────────────────────┐   ┌──────────────────────────────────┐
  │   ✓  OPTIMAL USE CASES (Do)      │   │   ✗  ANTI-PATTERNS (Do Not)      │
  │                                  │   │                                  │
  │   ✓ Repository refactoring       │   │   ✗ Product brainstorming        │
  │   ✓ Feature implementation       │   │   ✗ Architectural decisions      │
  │   ✓ Unit test generation         │   │   ✗ Vague architecture planning  │
  │   ✓ Debugging failing builds     │   │   ✗ Overly large transformations │
  │   ✓ Migration tasks              │   │                                  │
  │   ✓ Dependency upgrades          │   │                                  │
  │   ✓ CI/CD configuration          │   │                                  │
  └──────────────────────────────────┘   └──────────────────────────────────┘
```

The pattern is clear: Devin thrives on tasks with well-defined inputs, verifiable outputs, and bounded scope. It struggles with tasks that require subjective judgment, strategic thinking, or creative problem-solving — these remain firmly in the human domain.

---

## 10. Accelerate Codebase Exploration and Automate the Mundane

Devin serves two complementary modes of operation: exploration (understanding) and automation (execution). Both are valuable, and the best teams use them in sequence.

```
  ┌─────────────────────────────────┐   ┌──────────────────────────────────────┐
  │       EXPLORATION               │   │       AUTOMATION                     │
  │  Use Devin to onboard to        │   │  Generate repetitive tests           │
  │  new projects.                  │   │  or upgrades.                        │
  │                                 │   │                                      │
  │  ┌───────────────────────────┐  │   │  ┌────────────────────────────────┐  │
  │  │                           │  │   │  │                                │  │
  │  │  "Analyse this repository │  │   │  │  "Generate Mocha Chai unit     │  │
  │  │  and explain core         │  │   │  │  tests for checkoutHelpers.js. │  │
  │  │  architecture, key        │  │   │  │  Ensure coverage includes      │  │
  │  │  services, data flow,     │  │   │  │  loyalty voucher handling and  │  │
  │  │  and external             │  │   │  │  promotion logic."             │  │
  │  │  dependencies."           │  │   │  │                                │  │
  │  │                           │  │   │  └────────────────────────────────┘  │
  │  └───────────────────────────┘  │   │                                      │
  │                                 │   │  ┌────────────────────────────────┐  │
  │  Great for:                     │   │  │                                │  │
  │  • New team member onboarding   │   │  │  "Upgrade dependencies to      │  │
  │  • Understanding legacy code    │   │  │  latest compatible versions.   │  │
  │  • Pre-task reconnaissance      │   │  │  Ensure build passes."         │  │
  │                                 │   │  │                                │  │
  │                                 │   │  └────────────────────────────────┘  │
  │                                 │   │                                      │
  │                                 │   │  Great for:                          │
  │                                 │   │  • Bulk test generation              │
  │                                 │   │  • Dependency management             │
  │                                 │   │  • Repetitive code changes           │
  └─────────────────────────────────┘   └──────────────────────────────────────┘
```

Use exploration first to understand the codebase, then follow up with targeted automation sessions. The context gained during exploration makes your automation prompts dramatically more effective.

---

## 11. The Mandatory Human Validation Protocol

Devin can hallucinate dependencies and introduce incorrect assumptions. **Always validate output before committing.** No matter how confident Devin appears in its output, human verification is non-negotiable. This is your safety net against subtle issues that tests alone won't catch.

```
                         ┌─────────────────────────┐
                         │                         │
                         │    ⚠  DEV VERIFY FIX    │
                         │    BEFORE COMMIT.       │
                         │                         │
                         └────────┬────────────────┘
                    ┌─────────────┼─────────────┐
                    │             │             │
                    ▼             │             ▼
          ┌─────────────┐        │     ┌──────────────┐
          │ Unit Tests  │        │     │ Linting      │
          │             │        │     │ Tools        │
          │ Run the     │        │     │              │
          │ full test   │        │     │ Run linters  │
          │ suite.      │        │     │ to catch     │
          │             │        │     │ style and    │
          │             │        │     │ quality      │
          │             │        │     │ issues.      │
          └─────────────┘        │     └──────────────┘
                                 │
                    ┌────────────┴──────────────┐
                    │                           │
                    ▼                          ▼
          ┌──────────────┐          ┌──────────────────┐
          │ Security     │          │ Architecture     │
          │ Scanners     │          │ Reviews          │
          │              │          │                  │
          │ Run security │          │ Verify no        │
          │ scans to     │          │ unintended       │
          │ catch        │          │ structural       │
          │ vulnera-     │          │ changes were     │
          │ bilities.    │          │ introduced.      │
          └──────────────┘          └──────────────────┘
```

### Validation checklist before merging any Devin PR:

1. **Unit tests** — Run the full test suite, not just the tests Devin wrote
2. **Linting tools** — Confirm code meets your style and quality standards
3. **Security scanners** — Check for newly introduced vulnerabilities or dependency issues
4. **Architecture review** — Ensure no unintended structural changes slipped through

---

## 12. The Engineer's Devin AI Cheat Sheet

Treat Devin as an autonomous engineer that requires structured guidance and rigorous verification. These six principles summarise everything above into a quick reference:

```
  ┌───────────────────────┐  ┌───────────────────────┐  ┌───────────────────────┐
  │                       │  │                       │  │                       │
  │  1  PROVIDE CONTEXT   │  │  2  DEFINE CLEAR      │  │  3  SPECIFY           │
  │                       │  │     TASKS             │  │     CONSTRAINTS       │
  │  Point directly to    │  │                       │  │                       │
  │  the correct          │  │  Scope the objective  │  │  Dictate exactly      │
  │  repository and       │  │  explicitly.          │  │  what cannot be       │
  │  directory.           │  │                       │  │  changed.             │
  │                       │  │                       │  │                       │
  └───────────────────────┘  └───────────────────────┘  └───────────────────────┘

  ┌───────────────────────┐  ┌───────────────────────┐  ┌───────────────────────┐
  │                       │  │                       │  │                       │
  │  4  BREAK INTO        │  │  5  ANALYSE BEFORE    │  │  6  VALIDATE          │
  │     PHASES            │  │     EXECUTION         │  │     RESULTS           │
  │                       │  │                       │  │                       │
  │  Avoid massive        │  │  Use Ask Devin to     │  │  Verify fixes via     │
  │  rewrites; use        │  │  plan the prompt.     │  │  tests and linting    │
  │  iterative steps.     │  │                       │  │  before committing.   │
  │                       │  │                       │  │                       │
  └───────────────────────┘  └───────────────────────┘  └───────────────────────┘
```

---

## Quick Reference: Prompt Template

Use this template as a starting point for any Devin session:

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│  CONTEXT:                                                            │
│  [Describe the repository, technology stack, and relevant structure] │
│                                                                      │
│  TASK:                                                               │
│  [Clearly state what needs to be done]                               │
│                                                                      │
│  CONSTRAINTS:                                                        │
│  - Do not modify [specific files/modules/patterns]                   │
│  - Do not change [business logic / API contracts / schemas]          │
│  - Only work within [specific directory or scope]                    │
│                                                                      │
│  ACCEPTANCE CRITERIA:                                                │
│  - [Test command] passes with zero failures                          │
│  - [Lint command] produces no new warnings                           │
│  - [Specific verification step]                                      │
│                                                                      │
│  OUTPUT:                                                             │
│  - PR with descriptive summary                                       │
│  - List of changed files and rationale                               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```
