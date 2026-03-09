# Workshop 02: The Devin Workspace, DeepWiki & Ask Devin

**Duration: ~15 minutes**

## What You'll Learn

- What a Devin workspace is and what tools it provides
- How DeepWiki generates living documentation for your codebase
- How to steer DeepWiki for large repositories
- How to use Ask Devin to explore and understand code before starting a session
- The recommended workflow: Wiki → Ask Devin → Session

---

## 1. The Devin Workspace

Every time you start a Devin session, it spins up an **isolated workspace** in the cloud. This isn't just a chat window — it's a full development environment.

### What's Inside a Workspace

```
┌─────────────────────────────────────────────────────────────────┐
│                     Devin Workspace                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────┐  ┌──────────────┐  ┌───────────────────┐   │
│  │   Devin IDE     │  │   Terminal   │  │   Browser         │   │
│  │   (VS Code)     │  │   / Shell    │  │   (Interactive)   │   │
│  │                 │  │              │  │                   │   │
│  │ • Watch edits   │  │ • Run cmds   │  │ • Read docs       │   │
│  │   in real-time  │  │ • Install    │  │ • Test web apps   │   │
│  │ • Take over     │  │   deps       │  │ • Download/       │   │
│  │   and edit code │  │ • Run tests  │  │   upload files    │   │
│  │ • Navigate      │  │ • Build &    │  │ • Handle MFA/     │   │
│  │   the codebase  │  │   deploy     │  │   CAPTCHA         │   │
│  └─────────────────┘  └──────────────┘  └───────────────────┘   │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    Chat Interface                        │   │
│  │  Communicate with Devin, provide feedback, redirect      │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### The Three Workspace Tools

| Tool | What It Does | When You'd Use It |
|------|-------------|-------------------|
| **IDE (VS Code)** | Full VS Code editor embedded in the browser. You can watch Devin write code in real-time, or click in and edit files yourself. | Jump in to make a quick fix, review code as Devin writes it, or take over a task mid-session. |
| **Terminal / Shell** | Devin runs commands, installs packages, executes tests, and builds the project here. | Watch Devin install dependencies, check test output, or run a command yourself. |
| **Interactive Browser** | A browser Devin uses to read documentation, test web UIs, and navigate websites. You can take control of it. | Help Devin past a CAPTCHA or MFA prompt, verify a visual change, or test a web flow. |

### Sessions Are Self-Contained

Each session is isolated:
- Your prompt is treated as the "ticket" to resolve
- Devin drafts a plan, executes code, runs tests, and iterates — all within the same session
- You can **pause** at any point, **jump into** the IDE, or **let Devin finish** and open a PR
- Session state is never lost — you can come back hours later and pick up where you left off

> You don't need to babysit a session. Start it, move on to other work, and review the PR when Devin is done.

---

## 2. DeepWiki — Living Documentation

DeepWiki is Devin's code understanding engine. It automatically generates and continuously updates documentation for every indexed repository — architecture diagrams, dependency maps, and key concepts.

### What DeepWiki Provides

```
┌─────────────────────────────────────────────────────────────┐
│                      DeepWiki                               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │ Architecture    │  │ Dependencies    │                  │
│  │ Diagrams        │  │ & Imports       │                  │
│  └─────────────────┘  └─────────────────┘                  │
│                                                             │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │ Key Concepts    │  │ API Surface     │                  │
│  │ & Patterns      │  │ & Endpoints     │                  │
│  └─────────────────┘  └─────────────────┘                  │
│                                                             │
│  Powered by: Graph Algorithms + LLMs                        │
│  Updated: Continuously as code changes                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

| Feature | Description |
|---------|-------------|
| **Living documentation** | Continuously updated architecture diagrams, dependencies, and key concepts across all repos |
| **Faster onboarding** | Instantly understand unfamiliar code areas with accurate, auto-generated documentation |
| **Rich context for Devin** | Combines graph algorithms + LLMs to give Devin richer context, enabling sharper, more accurate code generation |

### How to Access DeepWiki

1. Navigate to **Wiki** in the left sidebar
2. Select the repository you want to explore (e.g., **PandoraJewelry/ai_workshop**)
3. Browse the auto-generated documentation pages

DeepWiki pages are organised by topic — you'll find sections for architecture overview, key modules, data models, API surface, and more.

### Steering DeepWiki for Large Repos

For large repositories, DeepWiki might not automatically focus on the parts that matter most. You can guide it by creating a `.devin/wiki.json` file in your repo:

```json
{
  "repo_notes": "This is a tea e-commerce store with a FastAPI backend and React/TypeScript frontend. Focus on the API endpoints, product models, customization system, and cart/wishlist features.",
  "pages": [
    {
      "title": "API Endpoints",
      "notes": "Document all FastAPI routes in backend/app/api/routes.py"
    },
    {
      "title": "Product Model & Customization",
      "notes": "Document the Product model and the customization config system in backend/app/"
    },
    {
      "title": "Frontend State Management",
      "notes": "Document CartContext, WishlistContext, and how state flows through React providers"
    }
  ]
}
```

This tells DeepWiki exactly which areas to prioritise, ensuring the most critical components are thoroughly documented.

---

## 3. Ask Devin — Intelligent Code Search

**Ask Devin** is a conversational search feature that leverages DeepWiki to answer questions about your codebase. Think of it as talking to a team member who has read every line of code.

### Why Use Ask Devin?

| Problem | How Ask Devin Solves It |
|---------|------------------------|
| "Where is the cart logic?" | Surfaces relevant files, contexts, and patterns instantly |
| "How do filters work in the API?" | Explains the flow across frontend and backend with file references |
| "What would it take to add a reviews feature?" | Analyses existing patterns and proposes an approach before you start a session |
| Fragmented search across Slack, Jira, docs | One interface for all technical questions about your codebase |

### How to Use Ask Devin

1. Click **Ask** in the left sidebar
2. Type your question in the chat box
3. Devin searches the Wiki and codebase to provide a contextual answer
4. If the answer leads to a task, you can start a session directly from the conversation

---

## 4. Hands-On: Ask Devin About the Tea Store

Now let's try Ask Devin with the workshop repository (**gravity9-tech/devin-pandora**). This is a tea e-commerce store with:

- **Backend:** Python/FastAPI with Pydantic models
- **Frontend:** React/TypeScript with Vite, Tailwind CSS
- **Features:** Product catalogue, filtering, shopping cart, wishlist, product customization

### Task 1: Understand the API Surface

Go to **Ask** and try these questions:

```
What API endpoints are available in this project and what do they do?
```

Devin should identify the endpoints defined in `backend/app/api/routes.py`:

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/products` | GET | List all products with optional filters (category, price_max, material) |
| `/api/products/{product_id}` | GET | Get a single product by ID |
| `/api/products/category/{category}` | GET | Get all products in a category (black, green, oolong, herbal) |
| `/api/customization-config/{category}` | GET | Get customization options and pricing for a tea category |
| `/health` | GET | Health check |

### Task 2: Explore the Data Model

```
What does the Product model look like and what fields does it have?
```

Devin should explain the `Product` model from `backend/app/models.py` — fields like `id`, `name`, `price`, `category` (black/green/oolong/herbal), `material` (China/Japan/India/Taiwan), `image`, `description`, and `customizable`.

### Task 3: Understand the Frontend State

```
How does the shopping cart work in the frontend? Where is the state managed?
```

Devin should walk you through:
- `CartContext.tsx` — React context that manages cart state
- `localStorage` persistence using the `pandora_cart` key
- Functions like `addItem`, `removeItem`, `updateQuantity`, `getTotal`
- How customized items are handled differently from regular items

### Task 4: Trace a Feature End-to-End

```
How does product filtering work from the UI all the way to the API?
```

This is a great question because it spans the full stack:
1. **Frontend:** `FilterSection` component captures user selections → updates `filters` state in `HomePage`
2. **Service layer:** `productService.ts` builds query parameters from filters
3. **API call:** `fetchApi` sends GET request to `/api/products?category=green&price_max=50`
4. **Backend:** `routes.py` applies filters to the product list and returns matching results

### Task 5: Explore the Customization System

```
How does the product customization system work? What options are available for different tea categories?
```

Devin should explain:
- Each tea category (black, green, oolong, herbal) has its own customization config in `backend/app/customization_config.py`
- Options include package size, brew strength/leaf style/roast level (varies by category), add-ons/accessories, and gift notes
- Each option has `price_modifier` values that affect the final price
- The frontend `CustomizationModal` component fetches config via `/api/customization-config/{category}` and renders the options dynamically

### Task 6: Plan Before Executing

Try using Ask Devin to plan a task before starting a session:

```
What would it take to add a product reviews feature to this project?
What files would need to change and what new files would be needed?
```

Devin should analyse the existing patterns and suggest:
- A new `Review` model in the backend
- New API endpoints for creating and listing reviews
- A frontend component for displaying and submitting reviews
- Updates to the `ProductCard` or a new product detail page

> This is the power of Ask Devin — you plan first, then execute with confidence. The answer becomes the basis for a well-scoped Devin session.

---

## 5. The Recommended Workflow

The most effective way to use Devin follows this pattern:

```
┌──────────┐         ┌──────────────┐         ┌─────────────┐
│          │         │              │         │             │
│   Wiki   │────────►│  Ask Devin   │────────►│   Session   │
│          │         │              │         │             │
└──────────┘         └──────────────┘         └─────────────┘
  Understand           Plan & scope             Execute
  the codebase         the task                 the work
```

| Step | What You Do | Why It Matters |
|------|-------------|----------------|
| **1. Wiki** | Browse DeepWiki to understand the relevant parts of the codebase | Builds your context before asking questions |
| **2. Ask Devin** | Ask questions, explore patterns, plan the approach | Ensures you have a clear, scoped task before starting |
| **3. Session** | Start a Devin session with a well-structured prompt | Higher success rate, lower cost, better PRs |

Skipping steps 1 and 2 is the most common mistake. Jumping straight to a session with a vague prompt leads to:
- Devin spending time exploring instead of building
- Broader, less focused PRs that are harder to review
- Higher ACU consumption for the same result

---

## Key Takeaways

1. **The Devin workspace** is a full cloud development environment — IDE, terminal, and browser — not just a chatbot
2. **DeepWiki** generates living documentation automatically; steer it with `.devin/wiki.json` for large repos
3. **Ask Devin** is your pre-session research tool — use it to understand code and plan tasks before executing
4. **Follow the Wiki → Ask → Session workflow** for the best results
5. **You can jump into any session** at any time — pause, edit, redirect, or take over without losing state

---

## Next Up

Continue to: [03_effective_prompting.md](./03_effective_prompting.md) — Learn how to write effective prompts and delegate tasks to Devin
