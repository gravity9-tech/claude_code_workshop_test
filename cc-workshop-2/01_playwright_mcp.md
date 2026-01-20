# Workshop 01: Playwright MCP

**Duration: ~10 minutes**

## What You'll Learn

- What Playwright MCP provides
- Verify Playwright is working (from dev-tools plugin)
- Start the pandora-demo application
- Test browser automation basics

---

## What is Playwright MCP?

**Playwright** is Microsoft's browser automation framework. The **Playwright MCP server** gives Claude the ability to:

- Navigate to URLs
- Click buttons and links
- Fill forms
- Take screenshots
- Extract text from pages
- Wait for elements
- Run JavaScript in the browser

This is essential for automated QA testing!

---

## How Playwright MCP Works

```
┌─────────────┐     MCP Protocol     ┌─────────────────┐
│ Claude Code │ ◄──────────────────► │ Playwright MCP  │
└─────────────┘                      └────────┬────────┘
                                              │
                                              ▼
                                     ┌─────────────────┐
                                     │  Real Browser   │
                                     │  (Chromium)     │
                                     └─────────────────┘
                                              │
                                              ▼
                                     ┌─────────────────┐
                                     │  Your App       │
                                     │  (localhost)    │
                                     └─────────────────┘
```

---

## Task 1: Verify Playwright MCP

The dev-tools plugin you installed in Workshop 1 includes Playwright MCP.

In Claude Code, check it's available:

```
/mcp
```

You should see both `jira` and `playwright` listed.

If Playwright is missing, you can add it manually:

```bash
claude mcp add playwright --scope project -- npx @playwright/mcp
```

---

## Task 2: Start the Application

Before we can test with Playwright, we need both servers running.

**Option 1: Start both at once**
```bash
cd pandora-demo
make dev
```

**Option 2: Start separately in two terminals**

**Terminal 1 - Backend:**
```bash
cd pandora-demo
make start-backend
# Runs on http://localhost:8000
```

**Terminal 2 - Frontend:**
```bash
cd pandora-demo
make start-frontend
# Runs on http://localhost:4210
```

**Access Points:**
| URL | Description |
|-----|-------------|
| http://localhost:4210 | Angular frontend (test here!) |
| http://localhost:8000 | FastAPI backend API |
| http://localhost:8000/docs | Swagger API docs |

**Keep these terminals running** — you'll need the app for testing.

---

## Task 3: Test Playwright Basics

Back in Claude Code, try some browser automation on the **frontend**:

```
Navigate to http://localhost:4210 and take a screenshot
```

Claude will:
1. Open a browser
2. Navigate to your app
3. Capture a screenshot
4. Show you the result

---

## Task 4: Interactive Testing

Try more complex interactions:

```
Go to http://localhost:4210, find all the products, and tell me their names and prices
```

```
Navigate to http://localhost:4210 and click on the first product
```

```
Go to http://localhost:4210 and add something to the basket
```

```
Go to http://localhost:4210 and toggle the dark mode
```

---

## Playwright MCP Tools

Here are the key tools you now have access to:

| Tool | What It Does |
|------|--------------|
| `browser_navigate` | Go to a URL |
| `browser_click` | Click an element |
| `browser_type` | Type into an input |
| `browser_fill_form` | Fill multiple fields |
| `browser_snapshot` | Get page accessibility tree |
| `browser_take_screenshot` | Capture image |
| `browser_wait_for` | Wait for element/condition |
| `browser_evaluate` | Run JavaScript |
| `browser_select_option` | Choose from dropdown |

---

## How This Enables QA Testing

In later modules, the `qa-ticket` skill will use Playwright to:

1. **Read BDD scenario** from QA subtask
2. **Execute the steps** via browser automation
3. **Verify outcomes** match expectations
4. **Take screenshots** as evidence
5. **Mark subtask PASS/FAIL**

Example:
```
Scenario: User adds product to wishlist
Given I am on the products page        → browser_navigate to :4210
When I click "Add to wishlist"         → browser_click
Then the item is in my wishlist        → browser_snapshot + verify
```

---

## Troubleshooting

**"Browser not installed":**
```
Install the Playwright browser
```
Or via terminal:
```bash
npx playwright install chromium
```

**"Connection refused":**
- Make sure both servers are running
- Frontend: `curl http://localhost:4210`
- Backend: `curl http://localhost:8000/health`

**Page loads but no products:**
- Backend might not be running
- Check `make start-backend` is active

**MCP not responding:**
- Restart Claude Code
- Check `/mcp` shows playwright

---

## Checkpoint

Before continuing, verify:

- [ ] `/mcp` shows playwright server
- [ ] Backend running at http://localhost:8000
- [ ] Frontend running at http://localhost:4210
- [ ] You can navigate to the frontend with Playwright
- [ ] Screenshots are being captured

---

## What You've Learned

1. **Playwright MCP** provides browser automation
2. **Key tools**: navigate, click, type, screenshot, wait
3. **Browser runs headless** (or visible) controlled by Claude
4. **Foundation for QA testing** of BDD scenarios
5. **Test the frontend** (port 4210) which talks to backend (port 8000)

---

## Next Up

Continue to: [02_slash_commands.md](./02_slash_commands.md) - Create reusable commands
