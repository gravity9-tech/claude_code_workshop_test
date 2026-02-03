# Workshop 00: Project Setup

**Duration: ~5 minutes**

## What You'll Do

- Clone the Tea Store project
- Understand the project structure
- Install dependencies
- Verify Claude Code installation

---

## Prerequisites

Before starting, ensure you have:

- **Git** installed
- **Python 3.9+** for the backend
- **Node.js 18+** for the frontend
- **Claude Code** installed (choose one):
  ```bash
  # Option 1: Using npm (all platforms)
  npm install -g @anthropic-ai/claude-code

  # Option 2: Using Homebrew (macOS/Linux only)
  brew install --cask claude-code
  ```
- A code editor (VS Code, Cursor, Vim, etc.)
- **Jira Cloud account** with:
  - A project you can create tickets in
  - An API token (we'll create this in Module 03)

---

## Step 1: Clone the Tea Store Project

Clone the full-stack e-commerce application we'll be working with:

```bash
git clone https://github.com/gravity9-tech/claude_code_workshop.git tea-store-demo
```

This creates a `tea-store-demo` folder containing a tea shop application.

---

## Step 2: Enter the Project

```bash
cd tea-store-demo
```

---

## Step 3: Open in Your Editor

**VS Code:**
```bash
code .
```

**Cursor:**
```bash
cursor .
```

**Vim:**
```bash
vim .
```

---

## Step 4: Verify Claude Code Installation

Test that Claude Code is installed within your text editor:

```bash
claude --version
```

You should see version output like `claude 2.x.x`.

---

## Step 5: Install & Run

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

## Project Features

The Tea Store app includes:

**Backend API:**
- `GET /api/products` — List products with filters
- `GET /api/products/{id}` — Get single product
- `GET /api/customization-config/{category}` — Customization options
- `GET /health` — Health check

**Frontend Features:**
- Product listing with hero section
- Filtering by category, price, material
- Shopping cart (localStorage)
- Wishlist functionality
- Product customization wizard
- Dark/light theme toggle
- Responsive design

---

## Useful Commands

| Command | Description |
|---------|-------------|
| `./start.sh` | Install deps & start both servers (macOS/Linux) |
| `start.bat` | Install deps & start both servers (Windows) |
| `./test.sh` | Run backend + frontend unit tests |
| `./test.sh --e2e` | Include E2E tests |
| `./test.sh --e2e --headed` | E2E with visible browser |
| `./test.sh --coverage` | Run with coverage reports |

> **Note:** On Windows, use `test.sh` with Git Bash or WSL.

---

## Checkpoint

Before continuing, verify:

- [ ] Tea Store repository cloned successfully
- [ ] Started the app with `./start.sh` (macOS/Linux) or `start.bat` (Windows)
- [ ] Backend running at http://localhost:8765
- [ ] Frontend running at http://localhost:4321
- [ ] `claude --version` works
- [ ] You have access to a Jira Cloud project

---

## Next Up

Continue to: [01_context_window.md](./01_context_window.md) - Understanding the Context Window
