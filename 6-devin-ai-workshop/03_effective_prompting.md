# Workshop 03: Effective Prompting & Best Practices

**Duration: ~15 minutes**

## What You'll Learn

- The golden rule of working with Devin: be specific
- How to avoid the "AI magic wand" trap
- How to structure prompts with context, requirements, and success criteria
- How to break complex projects into parallel, well-scoped sessions
- The do's and don'ts of delegating to Devin

---

## 1. The Golden Rule: Be Specific

The single most important thing when working with Devin is to **be as specific as possible**. Provide the same level of detail you would give a human colleague picking up a task for the first time.

The more specific and granular each session, the:
- **Higher** the chance of a successful, mergeable PR
- **Lower** the ACU (AI Compute Unit) consumption
- **Faster** the turnaround

Think of it this way: Devin is a junior engineer on their first week. They're capable and tireless, but they need clear direction. They won't guess your intent — they'll do exactly what you ask.

---

## 2. Avoid the "AI Magic Wand" Trap

The magic wand approach is the most common mistake — and the fastest road to frustration.

### What It Looks Like

```
"Hey Devin, here's my repo — convert it to TypeScript thanks!"
```

```
"Add a wishlist feature to the site."
```

```
"Find issues with our codebase and fix them."
```

### Why It Fails

```
┌─────────────────────────────────────────────────────────────────┐
│                  The Magic Wand Spiral                          │
│                                                                 │
│  Vague prompt                                                   │
│       │                                                         │
│       ▼                                                         │
│  Devin produces a vague, incomplete PR                          │
│       │                                                         │
│       ▼                                                         │
│  You nudge: "fix this one more thing"                           │
│       │                                                         │
│       ▼                                                         │
│  Session grows larger → performance deteriorates                │
│       │                                                         │
│       ▼                                                         │
│  More nudges → more cost → worse results                        │
│       │                                                         │
│       ▼                                                         │
│  Frustration. Low utility. Wasted ACUs.                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

The temptation to nudge Devin to fix "just one more thing" leads to growing session size, which means more cost and worse performance. Resist the urge — start a new, focused session instead.

---

## 3. The Anatomy of a Good Prompt

A well-structured prompt has four parts:

```
┌─────────────────────────────────────────────────────────────────┐
│                     Effective Prompt                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. TASK          What you want Devin to do (1-2 sentences)     │
│                                                                 │
│  2. CONTEXT       Tech stack, relevant files, existing patterns │
│                                                                 │
│  3. REQUIREMENTS  Specific deliverables and constraints         │
│                                                                 │
│  4. SUCCESS       How Devin verifies the work is done           │
│     CRITERIA                                                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

Let's break each part down.

### 3.1 Task — What to Do

State the task clearly in 1-2 sentences. Be direct:

| Bad | Good |
|-----|------|
| "Improve the API" | "Add a `GET /api/products/search` endpoint that supports full-text search on product name and description" |
| "Fix the frontend" | "Fix the cart total calculation — customized items with `price_modifier` values are not included in `getTotal()`" |
| "Update tests" | "Add unit tests for the `WishlistContext` — specifically `addItem`, `removeItem`, `toggleItem`, and `moveToCart`" |

### 3.2 Context — What Devin Needs to Know

Always give Devin materials to learn from. Reference existing files and patterns:

```
CONTEXT:
- Stack: Python/FastAPI backend, React/TypeScript frontend
- Existing cart implementation: see `frontend/src/contexts/CartContext.tsx` — use the same pattern
- Product model: see `backend/app/models.py`
- API routes follow the pattern in `backend/app/api/routes.py`
```

You can also link to external documentation:

```
CONTEXT:
- Follow the FastAPI query parameter docs: https://fastapi.tiangolo.com/tutorial/query-params/
- Reference the Pydantic v2 migration guide: https://docs.pydantic.dev/latest/migration/
```

> Devin can open links in its browser and read documentation. Use this to point it at API references, migration guides, or library docs.

### 3.3 Requirements — What Exactly to Deliver

Be explicit about what you expect:

```
REQUIREMENTS:
- New endpoint: GET /api/products/search?q=<query>
- Search should match against `name` and `description` fields (case-insensitive)
- Return a List[Product] (same response format as /api/products)
- Support optional `category` filter alongside search
- Return empty list (not 404) when no results match
```

### 3.4 Success Criteria — How to Verify

Tell Devin exactly how to confirm the work is done:

```
SUCCESS CRITERIA:
- All existing tests pass: `cd backend && python -m pytest`
- New tests cover the search endpoint (at least 3 test cases)
- No lint errors: `cd backend && flake8`
- Manual verification: GET /api/products/search?q=green returns only green tea products
```

> Commands in success criteria must complete within 5 minutes — Devin's lint and test runners have a timeout.

---

## 4. Good vs. Bad Prompts — Tea Store Examples

Let's see the difference with real examples from the workshop project.

### Example 1: Adding a New Feature

**Bad:**
> "Add a product reviews feature to the site."

Why it fails: No technical context, no data model guidance, no success criteria, no reference patterns.

**Good:**
> "Add a product reviews feature to the Tea Store.
>
> **CONTEXT:** Backend is FastAPI (see `backend/app/api/routes.py` for route patterns). Frontend is React/TypeScript (see `frontend/src/components/shared/ProductCard.tsx` for component patterns). Product model is in `backend/app/models.py`.
>
> **REQUIREMENTS:**
> - New `Review` Pydantic model with fields: `id`, `product_id`, `rating` (1-5), `comment` (max 500 chars), `author_name`, `created_at`
> - New endpoints: `POST /api/products/{product_id}/reviews` and `GET /api/products/{product_id}/reviews`
> - Store reviews in-memory (same pattern as `mock_data.py`)
> - Frontend: add a reviews section below the product grid on HomePage, showing average rating and review count per product
>
> **SUCCESS CRITERIA:**
> - Existing tests pass: `cd backend && python -m pytest`
> - New tests for both review endpoints (create and list)
> - No lint errors: `cd backend && flake8`"

### Example 2: Fixing a Bug

**Bad:**
> "The cart is broken, fix it."

**Good:**
> "Fix the cart quantity update bug in `CartContext.tsx`.
>
> **CONTEXT:** Cart state is managed in `frontend/src/contexts/CartContext.tsx`. The `updateQuantity` function is called from `CartSidebar.tsx`.
>
> **BUG:** When updating quantity of a customized item (where `isCustomized === true`), the function matches by `id` only, but customized items can share the same product `id` with different customization options. This causes all items with that product ID to update.
>
> **FIX:** Match customized items by both `id` and their `customizationSummary` to ensure only the intended item is updated.
>
> **SUCCESS CRITERIA:**
> - Existing cart tests pass: `cd frontend && npm test`
> - Add a test case for updating quantity of a customized item when multiple items share the same product ID"

### Example 3: Improving Test Coverage

**Bad:**
> "Write more tests."

**Good:**
> "Improve test coverage for the `productService.ts` module.
>
> **CONTEXT:** Service file: `frontend/src/services/productService.ts`. Existing test: `frontend/src/services/productService.test.ts`. Uses `fetchApi` from `frontend/src/services/api.ts`.
>
> **REQUIREMENTS:**
> - Add test cases for `getProducts` with each filter combination (category, priceMax, material)
> - Add test cases for `getProduct` including 404 error handling
> - Add test cases for `getProductsByCategory` with valid and invalid categories
> - Mock `fetchApi` — do not make real API calls
>
> **SUCCESS CRITERIA:**
> - All tests pass: `cd frontend && npm test`
> - At least 90% coverage for `productService.ts`"

---

## 5. Breaking Down Complex Tasks

Large tasks should never be a single session. Break them into smaller, well-scoped sessions and run them in parallel.

### The Pattern

```
┌─────────────────────────────────────────────────────────────────┐
│            Complex Task: JS → TypeScript Migration              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ❌ ONE BIG SESSION                                             │
│  "Convert the whole repo to TypeScript"                         │
│  → Vague, overwhelming, high failure rate                       │
│                                                                 │
│  ✅ MULTIPLE FOCUSED SESSIONS (in parallel)                     │
│                                                                 │
│  Session 1 (Planning)                                           │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ "Plan a phased migration. List all files that need      │    │
│  │  converting. Identify shared types and interfaces       │    │
│  │  that should be created first. Reference TS migration   │    │
│  │  best practices."                                       │    │
│  └─────────────────────────────────────────────────────────┘    │
│       │                                                         │
│       ▼                                                         │
│  Sessions 2, 3, 4… (Execution — run in parallel)               │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐         │
│  │ "Convert      │ │ "Convert      │ │ "Convert      │         │
│  │  Header       │ │  CartContext   │ │  ProductCard  │         │
│  │  component    │ │  and its      │ │  and its      │         │
│  │  and tests    │ │  tests to TS" │ │  tests to TS" │         │
│  │  to TS"       │ │               │ │               │         │
│  └───────────────┘ └───────────────┘ └───────────────┘         │
│       │                                                         │
│       ▼                                                         │
│  Session N (Testing)                                            │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ "Review test coverage. List components that need        │    │
│  │  additional tests. Ensure tsc compiles without errors." │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Why This Works

| Single Large Session | Multiple Focused Sessions |
|---------------------|--------------------------|
| Vague, broad scope | Clear, narrow scope per session |
| High failure rate | High success rate per session |
| Expensive (large context = more ACUs) | ACU-efficient (smaller context) |
| One big PR, hard to review | Small PRs, easy to review |
| If it fails, you lose everything | If one fails, the others still succeed |
| Slower (sequential work) | Faster (parallel execution) |

### Tea Store Example: Migrating from Static Mock Data to MongoDB

Instead of one session — "Replace all mock data with a MongoDB database" — break it down:

**Session 1:** "Set up MongoDB connection in `backend/app/`. Create a `database.py` module using `motor` (async MongoDB driver). Add a startup event in `backend/app/main.py` to connect to MongoDB. Add `motor` and `pymongo` to `requirements.txt`. Run `python -m pytest` to verify nothing breaks."

**Session 2:** "Migrate the product data from `backend/app/mock_data.py` to MongoDB. Create a `products` collection, write a seed script in `backend/scripts/seed_db.py` to insert the existing mock data, and update `backend/app/api/routes.py` to query MongoDB instead of importing from `mock_data.py`. Run `python -m pytest` to verify."

**Session 3:** "Add a `cart` collection in MongoDB. Create new endpoints `POST /api/cart`, `GET /api/cart/{user_id}`, and `DELETE /api/cart/{user_id}/items/{item_id}` in `backend/app/api/routes.py`. Follow the existing route patterns. Add tests for each endpoint. Run `python -m pytest` to verify."

Three parallel sessions, three focused PRs, three easy reviews.

---

## 6. Leveraging Context and References

Always give Devin materials to learn from — don't assume it will find the right patterns on its own.

### Reference Existing Patterns

```
"Reference the existing CartContext (frontend/src/contexts/CartContext.tsx)
for how we structure React contexts — same pattern with localStorage
persistence, Provider component, and custom hook."
```

```
"Follow the route structure in backend/app/api/routes.py — use the same
error handling pattern with HTTPException for invalid inputs."
```

### Link to External Documentation

```
"Follow the Playwright testing guide: https://playwright.dev/docs/writing-tests"
```

```
"Use the FastAPI dependency injection pattern from:
https://fastapi.tiangolo.com/tutorial/dependencies/"
```

Devin opens these links in its browser and reads the documentation. This is especially useful for:
- Library APIs Devin might not know the latest version of
- Internal documentation hosted on your company wiki
- Migration guides with step-by-step instructions

---

## 7. Feedback Loops and Session Insights

### Use Tests as Guardrails

Include test and lint commands in your prompts so Devin can self-verify:

```
Before opening a PR, verify:
1. All existing tests pass: cd frontend && npm test
2. Lint passes: cd frontend && npm run lint
3. TypeScript compiles: cd frontend && npx tsc --noEmit
```

Devin runs these commands, checks the output, and iterates if anything fails — without you needing to intervene.

### Review Session Insights

After a complex session, Devin provides **Session Insights** — an automated analysis that:
- Identifies what went well and what didn't
- Highlights technical problems Devin encountered
- Suggests improved prompt wording for future runs

Use these insights to refine your prompts over time. Your first prompt for a task type will rarely be your best — iteration is expected.

---

## 8. Do's and Don'ts Summary

### Do's

| Practice | Why |
|----------|-----|
| Treat Devin like a team of junior SWEs | Sets the right expectations — clear tasks, not open-ended problems |
| Use Devin to help plan and scope tasks | Ask Devin → Session workflow produces better results |
| Break down tasks into smaller chunks | One task per session, run multiple sessions in parallel |
| Reference existing code and patterns | "Follow the pattern in `CartContext.tsx`" beats "use React context" |
| Include success criteria with verification commands | Devin can self-check before opening a PR |
| Review Session Insights | Learn from each session and improve prompts over time |
| Build out Playbooks for repeated tasks | Standardise common workflows across the team |
| Run multiple sessions in parallel | 3 focused sessions > 1 bloated session |

### Don'ts

| Anti-Pattern | Why It Fails |
|-------------|-------------|
| Open-ended code review — "find issues and fix them" | No clear scope, Devin doesn't know when to stop |
| Vague, complex projects — "build microservices for our app" | Too broad, too many architectural decisions for a junior engineer |
| Large sessions with constant nudges and scope changes | Growing context degrades performance and wastes ACUs |
| Visual design tasks — "build exactly what's in this Figma" | Devin builds functional UIs but can't match pixel-perfect designs |
| Skipping the planning step | Jumping straight to a session without Ask Devin leads to vague prompts |
| Using one session for everything | Always split into parallel, focused sessions |

---

## 9. Hands-On: Write Your Own Prompt

Now it's your turn. Using what you've learned, write a prompt for one of these tasks on the Tea Store project. Follow the four-part structure (Task, Context, Requirements, Success Criteria):

### Option A: Add a Sort Feature
The product grid on the homepage currently has no sorting. Write a prompt to add sort-by-price (ascending/descending) and sort-by-name options.

**Hints to include in your prompt:**
- Frontend filter state is managed in `frontend/src/components/features/home/HomePage.tsx`
- Product list comes from `getProducts()` in `frontend/src/services/productService.ts`
- Sorting could be client-side (filter the existing array) or server-side (new query parameter on `GET /api/products`)

### Option B: Add an Order Summary Endpoint
The cart currently lives entirely in the frontend (localStorage). Write a prompt to add a backend endpoint that accepts a cart payload and returns an order summary with subtotal, tax, and total.

**Hints to include in your prompt:**
- Cart item type is defined in `frontend/src/types/cart.ts`
- Existing API routes in `backend/app/api/routes.py`
- The `Product` model in `backend/app/models.py` has price and customization fields

### Option C: Your Own Task
Pick any improvement to the Tea Store and write a well-structured prompt for it.

> After writing your prompt, paste it into **Ask Devin** to see how Devin interprets it and plans the work — before starting an actual session.

---

## Key Takeaways

1. **Be specific** — the golden rule. Provide the level of detail you'd give a new team member.
2. **Avoid the magic wand** — vague prompts lead to vague PRs, scope creep, and wasted ACUs.
3. **Structure every prompt** with Task, Context, Requirements, and Success Criteria.
4. **Break complex work into parallel sessions** — 3 focused sessions beat 1 bloated session every time.
5. **Reference existing code** — point Devin to patterns, files, and docs.
6. **Use tests as guardrails** — include verification commands so Devin can self-check.
7. **Iterate on your prompts** — review Session Insights and improve over time.

---

## Next Up

Continue to: [04_playbooks_workflows.md](./04_playbooks_workflows.md) — Create reusable Playbooks and automate multi-step workflows
