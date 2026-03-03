# Workshop 01: Getting Started with Devin

**Duration: ~20 minutes**

## What You'll Learn

- How to access Devin using your Pandora account
- Navigate the Devin dashboard and start a session
- Set up a repository on Devin's Machine (environment configuration)
- Configure dependencies, lint, tests, and local app commands
- Understand Knowledge, AGENTS.md, and Secrets

---

## Before You Begin

> You should have received an **invitation email** to Devin AI at your **Pandora email address** (e.g., `yourname@pandora.net`). If you haven't received it, check your spam folder or contact your team lead.

The invitation email contains a link to join your organisation's Devin workspace. Click the link and follow the prompts to activate your account.

---

## Step 1: Access Devin

Open your browser and navigate to:

```
https://app.devin.ai
```

Sign in with your Pandora credentials. You'll land on the **Devin Dashboard** — this is your home base for starting sessions, managing repositories, and reviewing Devin's work.

### Dashboard Overview

```
┌─────────────────────────────────────────────────────────────┐
│  Devin Dashboard                                            │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│  Navigation  │  Main Area                                   │
│              │                                              │
│  • Sessions  │  Start a new session, view active/past       │
│  • Ask       │  sessions, and monitor Devin's work.         │
│  • Wiki      │                                              │
│  • Review    │  Use the chat box to give Devin a task       │
│              │  or ask a question.                          │
│  • Settings  │                                              │
│              │                                              │
└──────────────┴──────────────────────────────────────────────┘
```

Key areas to know:

| Section | Purpose |
|---------|---------|
| **Sessions** | Start new sessions, view active and past sessions |
| **Wiki** | Auto-generated documentation for your indexed repos (DeepWiki) |
| **Settings** | Integrations (GitHub, Jira, etc.), team management |

---

## Step 2: Connect Your Repository

Before Devin can work on your code, it needs access to your repository and a properly configured environment. This is done through **Devin's Machine** — think of it as Devin's first day at work.

### 2.1 Grant Repository Access

1. Go to **Settings > Integrations**
2. Ensure GitHub is Enabled
3. Verify that the workshop repository is accessible:
   ```
   https://github.com/gravity9-tech/claude_code_workshop
   ```

### 2.2 Open Devin's Repositories

1. Navigate to **Repositories** in the left sidebar (under **Settings** group)
2. All available repos to your account will be displayed in the main area
3. Click **"Add to machine"**
3. Select **gravity9-tech/claude_code_workshop** from the list

This opens the **Repo Setup** wizard — an 8-step process that configures Devin's development environment for this repository.

---

## Step 3: Configure the Repository (8-Step Setup)

Devin's Machine runs on **Ubuntu 22.04 (x86_64)**. During setup, you configure the environment once — Devin saves a **snapshot** of the virtual machine so it doesn't have to repeat setup in future sessions.

> Getting this right significantly improves Devin's performance. A well-configured environment means Devin spends time coding, not debugging setup issues.

### The 8 Setup Steps

```
┌───────────────────────────────────────────────────────────────┐
│                    Repo Setup Wizard                          │
├───────────────────────────────────────────────────────────────┤
│                                                               │
│  Step 1: Git Pull                                             │
│  Step 2: Configure Secrets                                    │
│  Step 3: Install Dependencies                                 │
│  Step 4: Maintain Dependencies                                │
│  Step 5: Set up Lint                                          │
│  Step 6: Set up Tests                                         │
│  Step 7: Run Local App                                        │
│  Step 8: Additional Notes                                     │
│                                                               │
│  [ Finish Setup ]                [ Finish Later ]             │
└───────────────────────────────────────────────────────────────┘
```

Walk through each step:

### Step 1 — Git Pull

The command Devin runs at the start of every session to pull the latest changes. Keep the default unless your repo uses submodules that require additional access configuration.

### Step 2 — Configure Secrets

Add any API keys, tokens, or passwords that the project needs. We'll cover secrets in more detail in Step 5 of this workshop. For now, skip this if the workshop repo doesn't require external credentials.

### Step 3 — Install Dependencies

These commands run **once** during the initial environment setup. Use the VS Code terminal in the setup wizard to install everything the project needs:

```bash
# Example for this workshop repo
npm install
```

**Tip:** Use `Ctrl+K` in the terminal to let Devin auto-generate installation commands based on your project files.

### Step 4 — Maintain Dependencies

These commands run **after every git pull** on subsequent sessions to ensure new dependencies are picked up:

```bash
npm install
```

This way, if a teammate adds a new package, Devin's environment stays current.

### Step 5 — Set up Lint

Provide the lint command Devin runs **before committing changes** to ensure code quality:

```bash
npm run lint
```

Devin reviews the lint output and fixes issues before making commits.

> Commands must complete within **5 minutes** — keep them focused.

### Step 6 — Set up Tests

Provide the test command Devin runs **before committing changes**:

```bash
npm test
```

Devin monitors test results and iterates until tests pass before making a commit.

> Same 5-minute timeout applies. If your full test suite takes longer, scope it to relevant tests (e.g., `npm test -- --testPathPattern=<relevant-folder>`).

### Step 7 — Run Local App

Provide the command to start the application locally so Devin can test and debug using its integrated browser:

```bash
npm run dev
```

This enables Devin to verify changes visually — it can open the app in its browser, click through flows, and confirm things work.

### Step 8 — Additional Notes

Add any custom instructions for Devin when working on this repository. For example:

```
- Always create feature branches from `develop`, not `main`
- Use conventional commit messages (feat:, fix:, chore:)
- Run tests before pushing
```

### Finish Setup

Click **"Finish Setup"** to save the configuration. Devin will replay all commands and create an environment snapshot.

> If you need to step away, click **"Finish Later"** — your progress is saved. Note that only one setup can be in progress at a time.

---

## Step 4: Knowledge — Devin's Organisational Memory

**Knowledge** is a collection of organisation-wide instructions and advice that Devin automatically references across all sessions. Think of it as onboarding documentation for a new team member — except Devin never forgets it.

### How to Add Knowledge

1. Go to **Knowledge** in the sidebar (or **Settings > Knowledge**)
2. Click **"Add Knowledge"**
3. Fill in:
   - **Trigger Description:** A phrase that tells Devin when this knowledge is relevant (e.g., "when writing API endpoints", "when creating database migrations")
   - **Content:** The actual instruction or advice

### What to Include

| Topic | Example |
|-------|---------|
| **Coding standards** | "Always use TypeScript strict mode. Prefer `const` over `let`." |
| **Deployment steps** | "Run database migrations before deploying. Use `npm run migrate`." |
| **Common bug fixes** | "If the auth token expires, refresh it using the `/auth/refresh` endpoint." |
| **Internal tools** | "Use our internal `@pandora/logger` package for all logging." |
| **PR conventions** | "PR titles must follow conventional commits. Include ticket number." |

### Pinning Knowledge to Repositories

Knowledge can be scoped to:

| Scope | Behaviour |
|-------|-----------|
| **No repo** | Retrieved only when Devin detects relevance based on context |
| **Specific repo** | Always applied when working in that repository |
| **All repos** | Applied across every repository in your organisation |

**Tip:** Pin coding standards and deployment steps to **all repos**. Pin repo-specific quirks (like "this project uses a custom build script") to that **specific repo**.

### Knowledge Suggestions

Devin can automatically suggest knowledge items based on feedback from your sessions. When Devin proposes a suggestion, you can:
- Edit it before saving
- Request a regenerated version
- Dismiss it

Over time, this builds a growing library of institutional knowledge that makes Devin increasingly effective.

---

## Step 5: Secrets and Credentials

Devin handles credentials securely through three scopes:

```
┌────────────────────────────────────────────────────────────┐
│                    Secret Scopes                           │
├────────────────────┬───────────────────┬───────────────────┤
│  Global (Org)      │  Repo-Specific    │  Session-Specific │
├────────────────────┼───────────────────┼───────────────────┤
│ Managed via        │ Configured during │ Provided in chat  │
│ Secrets page       │ repo setup as     │ or requested by   │
│                    │ .env variables    │ Devin mid-session  │
│ Available to all   │                   │                   │
│ sessions org-wide  │ Scoped to that    │ Not saved for     │
│                    │ repo's snapshot   │ future sessions    │
│ Admin-managed      │                   │                   │
│ Encrypted at rest  │ Isolated from     │ Temporary only    │
│                    │ other repos       │                   │
└────────────────────┴───────────────────┴───────────────────┘
```

### Supported Secret Types

| Type | Use Case |
|------|----------|
| **Raw Secrets** | API keys, SSH keys, tokens, usernames, passwords |
| **Site Cookies** | Authentication tokens for automatic browser login |
| **TOTP (2FA)** | Time-based one-time passwords (like Google Authenticator) |

### Setting Up Secrets

1. Go to **Secrets** in the sidebar (or `https://app.devin.ai/secrets`)
2. Click **"Add Secret"**
3. Choose the type (Raw Secret, Site Cookie, or TOTP)
4. Enter the name, value, and optional notes

### Service Accounts (Recommended)

Create a dedicated service account for Devin's access to platforms:

```
devin@pandora.net
```

This keeps Devin's access separate from personal accounts and makes credential management cleaner.

### Handling 2FA / TOTP

If a service requires two-factor authentication:

1. Go to the service's security settings
2. Find the QR code for setting up an authenticator
3. Take a screenshot of the QR code
4. In Devin's Secrets page, select **"One-Time Password"** type
5. Upload the QR screenshot using the QR icon in the value field

Devin will generate TOTP codes automatically when needed.

> Only use Devin-specific accounts for 2FA — never share your personal 2FA codes.

---

## Step 6: AGENTS.md — Project-Level Instructions

`AGENTS.md` is an **open standard** file (like a README, but for AI agents) placed in your project's root directory. Devin reads it automatically at the start of every session.

Use it for project-specific guidance that goes beyond org-wide Knowledge.

### What to Include

Create an `AGENTS.md` file in the root of your repository:

```markdown
# AGENTS.md

## Setup Commands
- Install: `npm install`
- Dev server: `npm run dev`
- Tests: `npm test`
- Build: `npm run build`

## Code Style
- Use TypeScript strict mode
- Prefer functional components in React
- Follow ESLint + Prettier configuration in the repo
- Use conventional commit messages

## Testing Guidelines
- Write unit tests for all new functions
- Aim for >80% code coverage
- Run tests before every commit

## Project Structure
- `src/` — Application source code
- `tests/` — Test files
- `docs/` — Documentation

## Development Workflow
- Create feature branches from `develop`
- PR titles must include the Jira ticket number
- All PRs require at least one review before merge
```

### AGENTS.md vs Knowledge

| | AGENTS.md | Knowledge |
|---|-----------|-----------|
| **Scope** | Single repository | Organisation-wide or per-repo |
| **Location** | In the repo (version controlled) | In Devin's platform |
| **Maintained by** | Developers (via Git) | Team leads / admins (via UI) |
| **Best for** | Project-specific setup, structure, style | Cross-repo standards, deployment steps, common fixes |

Use **both** — they complement each other. AGENTS.md lives with the code, Knowledge lives in Devin's brain.

> Devin also auto-detects `.rules`, `.cursorrules`, `.windsurf`, and `CLAUDE.md` files, so your existing AI configuration files carry over.

---

## Checkpoint

At this point, you should have:

- [x] Signed into Devin at `https://app.devin.ai` with your Pandora account
- [x] Connected the workshop repository (`gravity9-tech/devin-pandora`)
- [x] Completed the 8-step Repo Setup wizard
- [x] Understood how Knowledge, Secrets, and AGENTS.md work together

```
┌──────────────────────────────────────────────────────────────┐
│              Your Devin Setup (Complete)                      │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  GitHub ──► Repository Access ──► Repo Setup Wizard          │
│                                       │                      │
│                    ┌──────────────────┼──────────────┐       │
│                    │                  │              │        │
│                    ▼                  ▼              ▼        │
│              ┌──────────┐    ┌──────────────┐  ┌─────────┐  │
│              │  Devin's │    │  Knowledge   │  │ Secrets │   │
│              │  Machine │    │  (org-wide)  │  │ (creds) │   │
│              │ (env +   │    └──────────────┘  └─────────┘  │
│              │  deps)   │           │                        │
│              └──────────┘           │                        │
│                    │                ▼                         │
│                    │        ┌──────────────┐                 │
│                    │        │  AGENTS.md   │                 │
│                    │        │ (in-repo)    │                 │
│                    │        └──────────────┘                 │
│                    │                │                         │
│                    └────────┬───────┘                         │
│                             ▼                                │
│                    ┌──────────────────┐                      │
│                    │  Devin Session   │                      │
│                    │  (ready to go!)  │                      │
│                    └──────────────────┘                      │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## Tips Before Your First Session

1. **Test your setup commands** — Use the VS Code terminal in the setup wizard to verify commands work before clicking "Finish Setup"
2. **Use absolute paths** or add executables to PATH when configuring commands
3. **Keep commands under 5 minutes** — Devin's lint and test commands have a timeout
4. **Version History** — If you break something during setup, go to Machine > Version History to restore a previous snapshot
5. **One setup at a time** — Only one repo setup can be in progress at a time; finish or save before starting another

---

## Next Up

Continue to: [02_deepwiki_ask_devin.md](./02_deepwiki_ask_devin.md) — Explore the Devin workspace (IDE, terminal, browser) and DeepWiki and Ask Devin features
