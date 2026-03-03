# Workshop 05: Integrations — Jira, GitHub & MCP

**Duration: ~15 minutes**

## What You'll Learn

- What integrations Devin supports and how to enable them
- How to connect Devin to Jira and trigger analysis from a ticket
- How to go from a Jira story to a Devin session to a merged PR
- What the MCP Marketplace is and how to add tools like Playwright

---

## 1. Devin's Integration Ecosystem

Devin connects to the tools your team already uses:

| Category | Integrations |
|----------|-------------|
| **Source Code** | GitHub (including PR templates), GitLab, Azure DevOps (Enterprise) |
| **Project Management** | Jira, Linear |
| **MCP Marketplace** | Datadog, Sentry, Slack, Notion, Playwright, Figma, PostgreSQL, and hundreds more |

All integrations are managed from **Settings > Integrations** in the Devin dashboard.

---

## 2. Jira Integration

The Jira integration lets you assign tickets to Devin and turn them into PRs — without leaving Jira.

### 2.1 Enable the Integration

Before using Jira with Devin, your team admin must connect a dedicated Jira account:

1. Create a dedicated Jira account for Devin (e.g., `devin@pandora.net`) — or use an email alias like `yourname+devin@pandora.net`
2. Set the account **display name** to **"Devin"** (this name appears on all comments)
3. Invite the account to your Jira workspace
4. In Devin, go to **Settings > Integrations** and connect the Jira account
5. Make sure you are logged into the **Devin Jira account** (not your personal one) during the connection step

> Only add the Devin Jira account to **one** Jira workspace.

### 2.2 How the Jira Workflow Works

```
┌──────────────┐       ┌──────────────────┐       ┌──────────────┐
│  Create Jira │       │  Devin analyses   │       │  Start Devin │
│  ticket +    │──────►│  ticket & posts   │──────►│  session     │
│  add "Devin" │       │  a comment with   │       │  from Jira   │
│  label       │       │  implementation   │       │  or dashboard │
│              │       │  plan             │       │              │
└──────────────┘       └──────────────────┘       └──────────────┘
     You                    ~5 min                     You
```

When you add the **"Devin"** label to a Jira ticket:

1. **Within ~1 minute** — Devin posts a preliminary response
2. **Within ~5 minutes** — Devin posts a detailed comment with:
   - Relevant code summary
   - Implementation strategy
   - Edge cases and clarifications
   - Confidence indicators (red / amber / green)
3. **You review** — If happy, click the link in the comment to start a session, or copy the ticket URL and paste it into the Devin dashboard

Devin **will not** start working until you explicitly tell it to. The label only triggers analysis, not execution.

> You can bulk-add the "Devin" label to multiple tickets to scope them in parallel.

---

## 3. Hands-On: Create a Jira Story for the Tea Store

Let's walk through the full workflow using a real task on the Tea Store project.

### 3.1 Create the Jira Ticket

Create a new **Story** in your Jira project with the following content:

**Title:**
```
Add text search filter by tea name to the homepage filter section
```

**Description:**
```
As a customer, I want to search for teas by name so that I can quickly
find a specific product without browsing the full catalogue.

## Context
The homepage has a filter section (FilterSection component) with dropdowns
for Tea Type, Price Range, and Origin. There is currently no way to search
by product name.

- Frontend filter component: frontend/src/components/features/home/FilterSection.tsx
- Filter types: frontend/src/types/filter.ts (ProductFilters interface)
- Product service: frontend/src/services/productService.ts
- Backend API: backend/app/api/routes.py (GET /api/products endpoint)
- Product model: backend/app/models.py

## Requirements
- Add a text input field to the FilterSection component, placed before
  the existing Tea Type dropdown
- The field should have placeholder text: "Search by tea name..."
- Filter products where the name contains the search text (case-insensitive)
- Add a new optional query parameter `name` to the GET /api/products endpoint
- Update the ProductFilters type to include a `name: string | null` field
- Filtering should work in combination with existing filters
  (category, price, origin)
- Clear All Filters button should also clear the search text

## Acceptance Criteria
- [ ] Text input appears in the filter section above the Tea Type dropdown
- [ ] Typing "Dragon" shows only products with "Dragon" in the name
- [ ] Search works alongside other filters (e.g., name + category)
- [ ] Clear All Filters resets the search field
- [ ] Backend supports `name` query parameter on GET /api/products
- [ ] Existing tests still pass
- [ ] New tests cover the name filter on both frontend and backend
```

**Story Points:** 3

### 3.2 Add the "Devin" Label

1. Open the ticket you just created
2. In the **Labels** field, add the label **`Devin`** (create it if it doesn't exist)
3. Save the ticket

Within a few minutes, Devin will post a comment on the ticket with its analysis — relevant files, proposed implementation plan, edge cases, and a confidence score.

### 3.3 Review Devin's Analysis

Read the comment Devin posts on the ticket. Check:

- Did Devin identify the right files?
- Does the implementation plan match what you expect?
- Are there edge cases you didn't think of?

If something is off, reply to the comment with clarification. Devin will update its analysis.

### 3.4 Start a Devin Session

Once you're satisfied with the analysis, you have two options:

**Option A — Start from Jira:**
Click the **"Start Session"** link in Devin's comment on the ticket. This launches a session pre-loaded with the ticket context.

**Option B — Start from the Devin dashboard:**
1. Go to `https://app.devin.ai`
2. In the session launcher, paste the Jira ticket URL
3. Add implementation instructions (see below)
4. Launch the session

### 3.5 Provide Implementation Instructions

Whether starting from Jira or the dashboard, add these instructions to guide Devin:

```
Implement the Jira ticket: [paste your ticket URL here]

CONTEXT:
- The filter section is in frontend/src/components/features/home/FilterSection.tsx
- Filters state is managed in HomePage.tsx and passed down as props
- The ProductFilters type in frontend/src/types/filter.ts needs a new `name` field
- The backend GET /api/products endpoint in backend/app/api/routes.py
  needs a new optional `name` query parameter
- Use the existing filter pattern — see how `category` and `material`
  filters work end-to-end

REQUIREMENTS:
- Add a text input to FilterSection for searching by tea name
- Add `name` query parameter to GET /api/products (case-insensitive
  substring match on the product name field)
- Update ProductFilters type and productService.ts to pass the name filter
- Write backend tests in backend/tests/ for the new name parameter
- Write frontend tests for the updated FilterSection component

TESTING:
- Use the Playwright MCP server to run end-to-end tests after implementation
- Verify the search input renders on the homepage
- Verify that typing a tea name filters the product grid correctly
- Verify the Clear All Filters button resets the search field
- Verify combined filters work (e.g., name "Dragon" + category "green")

SUCCESS CRITERIA:
- All existing tests pass: cd backend && python -m pytest
- All existing frontend tests pass: cd frontend && npm test
- Lint passes: cd backend && flake8
- Playwright e2e tests pass for the new search filter feature
```

> Devin uses the **Playwright MCP server** to launch a browser, navigate to the running app, and test the feature end-to-end — just like a QA engineer would.

---

## 4. MCP Marketplace

MCP (Model Context Protocol) is an open standard that lets Devin connect to external tools and data sources. Think of it as a **USB-C port for AI** — one standard, hundreds of integrations.

### Enabling MCP Servers

1. Go to **Settings > MCP Marketplace**
2. Browse or search for the tool you need
3. Click to enable — some require credentials, others are one-click

### Commonly Used MCP Servers

| MCP Server | What It Does |
|------------|-------------|
| **Playwright** | Browser automation for e2e testing — Devin can navigate your app, click buttons, fill forms, and verify UI behaviour |
| **Sentry** | Monitor errors and exceptions — Devin can investigate production issues |
| **Datadog** | Access logs and metrics — Devin can diagnose performance problems |
| **Slack** | Read and send messages — Devin can post updates to channels |
| **Notion** | Read and create pages — Devin can update documentation |
| **PostgreSQL** | Query databases — Devin can inspect data and write migrations |

### Playwright MCP for Testing

The Playwright MCP server is particularly useful. When enabled, Devin can:

- Launch a browser in its workspace
- Navigate to your running application
- Interact with UI elements (click, type, select)
- Assert that elements are visible and contain expected content
- Take screenshots for verification

This is what we asked Devin to use in the Tea Store task above — it runs the app locally, opens the browser, and tests the search filter end-to-end.

To enable it: **Settings > MCP Marketplace > Playwright** — provide any required credentials and toggle it on.

---

## Key Takeaways

1. **Jira integration** turns tickets into Devin sessions — add the "Devin" label to trigger analysis
2. **Devin analyses before acting** — it posts a plan and waits for your approval before starting work
3. **Include implementation instructions** when starting a session — context, requirements, and success criteria still matter
4. **MCP Marketplace** extends Devin with hundreds of tools — Playwright for e2e testing, Sentry for error triage, Slack for updates
5. **Playwright MCP** lets Devin test your UI end-to-end, like a QA engineer with a browser

---

## Next Up

Continue to: [06_session_management.md](./06_session_management.md) — Run parallel sessions, review PRs, and manage Devin's work at scale
