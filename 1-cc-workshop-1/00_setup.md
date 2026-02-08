# Workshop 00: Project Setup

**Duration: ~5 minutes**

## What You'll Do

- Download the Tea Store project
- Understand the project structure
- Install dependencies
- Verify Claude Code installation

---

## Step 1: Download and Open the Tea Store Project

1. Download the project zip file:
   **https://github.com/gravity9-tech/claude_code_workshop/archive/refs/heads/main.zip**

2. Extract the zip file to a location of your choice

3. Open the extracted folder in your favorite text editor

---

## Step 2: Verify Claude Code Installation

Open a terminal in your project folder and run:

```bash
claude --version
```

You should see version output like `claude 2.x.x`.

---

## Step 3: Install & Run

In the terminal, run:

```bash
# macOS/Linux
./start.sh

# Windows
start.bat
```

This will install all dependencies and start both servers.

**Access Points:**
| URL | Description |
|-----|-------------|
| http://localhost:4321 | React frontend |
| http://localhost:8765 | FastAPI backend |
| http://localhost:8765/docs | API Docs |

---

## What We're Building

In this workshop, you'll build a complete Jira-driven development lifecycle:

```
Project Setup → Context Window → Getting Started
                                       │
                                       ▼
                              ┌──────────────┐
                              │   Jira MCP   │  ← Tool (capability layer)
                              └──────┬───────┘
                                     │
          ┌──────────┬───────────────┼───────────────┐
          ▼          ▼               ▼               ▼
   ┌────────────┐ ┌────────────┐ ┌─────────────┐ ┌─────────┐
   │  create-   │ │  expand-   │ │ implement-  │ │   qa-   │  ← Skills
   │  ticket    │ │  ticket    │ │ ticket      │ │  ticket │    (knowledge
   │            │ │            │ │             │ │         │     layer)
   └─────┬──────┘ └─────┬──────┘ └──────┬──────┘ └────┬────┘
         │              │               │              │
         └──────────────┼───────────────┼──────────────┘
                        ▼               ▼
               ┌─────────────────────────────┐
               │      /deploy-ticket         │  ← Command (orchestration)
               └─────────────────────────────┘
```

**Tool (capability layer):** Jira MCP gives Claude the ability to create, search, and transition tickets.

**Skills (knowledge layer):** Four skills each handle one phase of the lifecycle:
- `create-ticket` — Creates tickets with BDD acceptance criteria
- `expand-ticket` — Breaks tickets into DEV and QA subtasks
- `implement-ticket` — Implements code and transitions DEV tasks to Done
- `qa-ticket` — Runs Playwright tests and transitions QA tasks to Done

**Command (orchestration):** `/deploy-ticket` chains all four skills into a single workflow.

---

## Next Up

Continue to: [01_context_window.md](./01_context_window.md) - Understanding the Context Window
