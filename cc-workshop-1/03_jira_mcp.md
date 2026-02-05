# Workshop 03: Jira MCP Integration

**Duration: ~15 minutes**

## What You'll Learn

- What MCP (Model Context Protocol) is
- How to create Jira API credentials
- Verify the connection works

---

## What is MCP?

**Model Context Protocol (MCP)** is an open standard for connecting AI tools to external services. MCP servers give Claude Code new capabilities:

- **Jira** — Create and manage tickets
- **Playwright** — Browser automation (Workshop 2)
- **Databases** — Query and modify data
- **GitHub** — Manage repos and PRs

Think of MCP servers as **plugins** that extend what Claude can do.

---

## How MCP Works

```
┌─────────────┐     MCP Protocol     ┌─────────────────┐
│ Claude Code │ ◄──────────────────► │   MCP Server    │
└─────────────┘      (stdio)         │  (mcp-atlassian)│
                                     ├─────────────────┤
                                     │     Tools:      │
                                     │  • jira_search  │
                                     │  • jira_create  │
                                     │  • jira_update  │
                                     └────────┬────────┘
                                              │ REST API
                                              ▼
                                     ┌─────────────────┐
                                     │  Jira Cloud     │
                                     └─────────────────┘
```

MCP servers run as separate processes. Claude Code starts them automatically when needed.

---

## Task 1: Get Your Jira API Token

Before connecting to Jira, you need an API token:

1. Go to: https://id.atlassian.net/manage-profile/security/api-tokens
2. Click **"Create API token"**
3. Name it `Claude Code Workshop`
4. **Copy the token** — you won't see it again!

Keep this token handy for the next step.

---

## Task 2: Set Environment Variables

The project includes an `env.example` file with placeholder values.

**1. Copy and rename it to `.env`:**

```bash
cp env.example .env
```

**2. Edit `.env` and replace the placeholder values with your real credentials:**

```
JIRA_HOST=https://your-domain.atlassian.net
JIRA_EMAIL=your-email@example.com
JIRA_API_TOKEN=your-api-token-here
```

---

## Task 3: Install Jira MCP Server

Use our meta-skill to create the Jira MCP server configuration:

```
/create-jira-mcp create jira mcp server for this project
```

This will set up the MCP server configuration automatically.

Once complete, verify the connection by checking that Jira MCP is registered:

```
/mcp
```

You should see `jira` listed with status "connected" or "running".

Now test it:

```
List all projects I have access to in Jira
```

If successful, you'll see your Jira projects listed.

---

## Try It Out

Ask Claude to interact with your Jira:

```
Show me open bugs in project {Project Name}
```

```
What's the status of {Ticket-ID}?
```

```
Create a task in {Project Name}: "Test MCP integration"
```

---

**"Authentication failed":**
- Verify JIRA_HOST is just the domain (e.g., `your-domain.atlassian.net`, not `https://...`)
- Check that JIRA_EMAIL is your Atlassian account email
- Confirm the API token is correct (regenerate if unsure)

**Node.js issues:**
```bash
# Ensure Node.js is installed
node --version  # Should be v18+
```

---

## Checkpoint

Before continuing, verify:

- [ ] Jira API token created
- [ ] `.env` file created from `env.example` with your credentials
- [ ] `/mcp` shows jira server as connected
- [ ] Can list projects or search issues

---

## What You've Learned

1. **MCP** extends Claude Code with external service integrations
2. **`.env` files** keep credentials secure and easy to manage (never commit them!)
3. **mcp-atlassian** is a community package providing Jira and Confluence tools

---

## Next Up

Continue to: [04_skills_intro.md](./04_skills_intro.md) - Understanding Skills
