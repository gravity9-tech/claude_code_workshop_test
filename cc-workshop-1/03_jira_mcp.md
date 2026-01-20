# Workshop 03: Jira MCP Integration

**Duration: ~15 minutes**

## What You'll Learn

- What MCP (Model Context Protocol) is
- How to create Jira API credentials
- Add a Jira MCP server to Claude Code
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

Set your Jira credentials as environment variables. This keeps them secure and out of command history.

**In your terminal:**

```bash
export JIRA_HOST="https://your-domain.atlassian.net"
export JIRA_EMAIL="your-email@example.com"
export JIRA_API_TOKEN="your-api-token-here"
```

**To make these permanent**, add them to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
# Add to your shell profile
echo 'export JIRA_HOST="https://your-domain.atlassian.net"' >> ~/.zshrc
echo 'export JIRA_EMAIL="your-email@example.com"' >> ~/.zshrc
echo 'export JIRA_API_TOKEN="your-api-token-here"' >> ~/.zshrc

# Reload the profile
source ~/.zshrc
```

**Verify they're set:**

```bash
echo $JIRA_HOST
echo $JIRA_EMAIL
echo $JIRA_API_TOKEN
```

---

## Task 3: Install the MCP Server Locally

The `mcp-atlassian` package requires a local installation with its dependencies. Run this in your project root:

```bash
# Create a local MCP servers directory
mkdir -p .mcp-servers/jira && cd .mcp-servers/jira

# Initialize and install with required dependencies
npm init -y && npm install mcp-atlassian jsdom

# Return to project root and add to .gitignore (contains node_modules)
cd ../.. && echo ".mcp-servers/" >> .gitignore
```

---

## Task 4: Add the MCP Server

Use the `claude mcp add-json` command to register the Jira MCP server:

```bash
claude mcp add-json jira '{
    "type": "stdio",
    "command": "node",
    "args": [".mcp-servers/jira/node_modules/mcp-atlassian/dist/index.js"],
    "env": {
      "ATLASSIAN_BASE_URL": "${JIRA_HOST}",
      "ATLASSIAN_EMAIL": "${JIRA_EMAIL}",
      "ATLASSIAN_API_TOKEN": "${JIRA_API_TOKEN}"
    }
}' -s project
```
---

## Task 5: Verify the Connection

Start (or restart) Claude Code:

```bash
claude
```

Check that Jira MCP is registered:

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
- [ ] Environment variables set (JIRA_HOST, JIRA_EMAIL, JIRA_API_TOKEN)
- [ ] Local installation created in `.mcp-servers/jira/`
- [ ] MCP server added via `claude mcp add-json`
- [ ] `/mcp` shows jira server as connected
- [ ] Can list projects or search issues

---

## What You've Learned

1. **MCP** extends Claude Code with external service integrations
2. **Environment variables** keep credentials secure (never commit them!)
3. **`claude mcp add-json`** configures MCP servers per-project
4. **Local installation** ensures dependencies are properly resolved
5. **mcp-atlassian** is a community package providing Jira and Confluence tools

---

## Next Up

Continue to: [04_skills_intro.md](./04_skills_intro.md) - Understanding Skills
