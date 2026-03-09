# Workshop 04: Playbooks & Workflows

**Duration: ~10 minutes**

## What You'll Learn

- What Playbooks are and when to use them
- The difference between Playbooks and Knowledge
- How to create, structure, and use a Playbook
- How Devin's interactive planning and approval flow works

---

## 1. What Are Playbooks?

Playbooks are **reusable, shareable prompt templates** for repeated tasks. Think of them as custom system prompts — they tell Devin exactly how to approach a specific type of work, every time.

```
Without Playbook                    With Playbook
────────────────                    ──────────────
You write the same detailed         You attach a Playbook once.
prompt every time you want          Devin follows the same steps
Devin to do a migration,            consistently across every
a test coverage task, or            session — for every team
a dependency upgrade.               member who uses it.
```

### When to Use a Playbook

- You find yourself **repeating the same prompt** across multiple sessions
- A task follows a **consistent set of steps** (e.g., dependency upgrades, test scaffolding, API endpoint creation)
- You want **consistency across team members** — everyone delegates the same way

### Playbooks vs Knowledge

| | Playbooks | Knowledge |
|---|-----------|-----------|
| **Purpose** | Step-by-step task instructions | General tips, standards, and advice |
| **Scope** | One specific workflow | Organisation-wide or repo-wide |
| **When used** | Attached to a session at launch | Auto-recalled when Devin detects relevance |
| **Analogy** | A recipe card | A cooking guidebook |

Use **Knowledge** for: coding standards, deployment steps, PR conventions.
Use **Playbooks** for: "how to add a new API endpoint", "how to run a migration", "how to scaffold tests".

---

## 2. Playbook Structure

A good Playbook has five sections:

```
┌─────────────────────────────────────────────────────────┐
│                    Playbook                              │
├─────────────────────────────────────────────────────────┤
│  PROCEDURE        Step-by-step actions (imperative)     │
│  SPECIFICATIONS   What should be true when done         │
│  ADVICE           Tips to guide Devin's decisions       │
│  FORBIDDEN        Actions Devin must never take         │
│  REQUIRED INPUT   What the user must provide            │
└─────────────────────────────────────────────────────────┘
```

| Section | Required? | Description |
|---------|-----------|-------------|
| **Procedure** | Yes | One step per line, written imperatively. Cover setup, the task itself, and delivery. |
| **Specifications** | Recommended | Postconditions — what should be true after completion. |
| **Advice** | Optional | Tips to correct Devin's assumptions or guide it down the most efficient path. |
| **Forbidden Actions** | Optional | Actions Devin should absolutely not take. |
| **Required from User** | Optional | Input the user must provide when starting the session (e.g., endpoint name, category). |

### Writing Good Procedures

- **One step per line**, each written as an imperative action ("Write…", "Run…", "Navigate to…")
- Steps should be **mutually exclusive and collectively exhaustive** — no overlaps, no gaps
- Include at least one step for **setup**, the **actual task**, and **verification**
- Avoid being so specific that Devin can't problem-solve; include specific commands and strings where they matter

---

## 3. Example: "Add API Endpoint" Playbook

Here's a Playbook for adding a new API endpoint to the Tea Store project, based on the patterns in `backend/app/api/routes.py` and `backend/app/models.py`:

```markdown
# Add API Endpoint

## Required from User
- Endpoint path (e.g., `/api/products/search`)
- HTTP method (GET, POST, PUT, DELETE)
- Brief description of what the endpoint does
- Request parameters or body schema
- Response schema

## Procedure
1. Read the existing route patterns in `backend/app/api/routes.py`
   and the model definitions in `backend/app/models.py`.
2. Create any new Pydantic models needed for the request/response
   in `backend/app/models.py`. Follow the existing field annotation
   style with `Field(...)` descriptions.
3. Add the new route function in `backend/app/api/routes.py`.
   Use `Optional[...] = Query(...)` for query parameters.
   Validate inputs and raise `HTTPException(status_code=400)`
   for invalid values — follow the pattern of `get_products()`.
4. Write pytest tests for the new endpoint in `backend/tests/`.
   Cover: success case, validation errors, and edge cases
   (empty results, not found).
5. Run `cd backend && python -m pytest` and fix any failures.
6. Run `cd backend && flake8` and fix any lint errors.
7. Open a PR with a clear title and summary describing the
   new endpoint, its parameters, and response format.

## Specifications
- All existing tests still pass.
- New endpoint returns proper HTTP status codes (200, 400, 404).
- New Pydantic models include field descriptions.
- At least 3 test cases for the new endpoint.

## Advice
- Use the `get_products` function as a reference for query
  parameter filtering patterns.
- If the endpoint needs new mock data, add it to
  `backend/app/mock_data.py` following the existing format.
- Keep response models consistent with existing ones — reuse
  the `Product` model where possible.

## Forbidden Actions
- Do not modify existing endpoint behaviour.
- Do not change the API prefix (`/api`).
- Do not install new dependencies without stating why.
```

---

## 4. How to Use a Playbook

### Creating a Playbook

**Option A — In the web app:**
1. Go to the **Playbooks** section (under Settings or the session launcher)
2. Click **"Create a new Playbook"**
3. Paste your Playbook content
4. Give it a name (e.g., "Add API Endpoint")
5. Save

**Option B — As a file:**
1. Create a `.devin.md` file with your Playbook content
2. When starting a new session, **drag and drop** the file into the session launcher

### Attaching a Playbook to a Session

1. Start a new session from the dashboard
2. In the session launcher, look for the Playbook selector
3. Select your Playbook — it appears as a **blue pill** in the prompt area
4. Fill in any **Required from User** fields (e.g., endpoint path, method)
5. Add any session-specific context if needed
6. Launch the session

Devin reads the Playbook as its guiding instructions and follows the procedure step by step.

### Example: Using the Playbook to Add a Product Rating Endpoint

Attach the **Add API Endpoint** Playbook and provide the required inputs:

- **Endpoint path:** `/api/products/{product_id}/rating`
- **HTTP method:** POST
- **Description:** Submit a rating (1–5) for a specific product and return the updated average rating
- **Request body schema:** `{ "rating": int }` — value between 1 and 5
- **Response schema:** `{ "product_id": str, "average_rating": float, "total_ratings": int }`

### Iterating on Playbooks

Run **2+ Devin sessions in parallel** with the same Playbook to quickly identify gaps or errors in your instructions. Compare the results and refine the Playbook before rolling it out to the team.

---

## 5. Workflow Management: Planning & Approval

When a session starts, Devin doesn't jump straight into coding. It follows a planning flow:

```
Session Start
     │
     ▼
Devin assesses the codebase
     │
     ▼
Devin proposes a plan
     │
     ├──► Auto-execute (default)
     │
     └──► "Wait for my approval" (you review first)
              │
              ▼
         Review plan → Approve / Modify / Reject
              │
              ▼
         Devin executes
```

For complex or high-risk tasks, click **"Wait for my approval"** before Devin starts executing. This lets you:
- Review the proposed plan
- Suggest modifications
- Brainstorm alternatives
- Ensure Devin understood the requirements correctly

> Use approval mode for anything that touches shared infrastructure, modifies database schemas, or affects production configs.

---

## Key Takeaways

1. **Playbooks are reusable prompt templates** — write once, use across sessions and team members
2. **Structure matters** — Procedure, Specifications, Advice, Forbidden Actions, Required Input
3. **Playbooks are for tasks, Knowledge is for standards** — don't mix the two
4. **Iterate with parallel sessions** — run the same Playbook in 2+ sessions to find gaps
5. **Use approval mode** for complex tasks — review Devin's plan before it executes

---

## Next Up

Continue to: [05_integrations.md](./05_integrations.md) — Connect Devin to GitHub, Jira, and external tools via MCP
