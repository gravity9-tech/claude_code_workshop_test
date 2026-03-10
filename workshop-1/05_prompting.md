# Workshop 05: Prompting Best Practices

**Duration: ~10 minutes**

## What You'll Learn

- How to write prompts that get better results on the first try
- Using `@` references, images, and piped data
- Giving Claude verification criteria

---

## The #1 Rule: Give Claude a Way to Verify

Claude performs dramatically better when it can **check its own work**. Without verification, you become the only feedback loop.

| Without verification | With verification |
|---------------------|-------------------|
| "Add email validation" | "Add email validation. Test cases: user@example.com → true, invalid → false, user@.com → false. Run the tests after." |
| "Fix the build" | "The build fails with this error: [paste error]. Fix it and verify the build succeeds." |
| "Make the dashboard look better" | "Implement this design [paste screenshot]. Take a screenshot and compare." |

---

## Be Specific, Not Vague

Every vague prompt costs you tokens in corrections. Specific prompts get it right the first time.

| Vague (wastes tokens) | Specific (saves tokens) |
|----------------------|------------------------|
| "Add tests for foo.py" | "Write a test for foo.py covering the edge case where the user is logged out. Avoid mocks." |
| "Fix the login bug" | "Login fails after session timeout. Check the auth flow in src/auth/, especially token refresh." |
| "Add a calendar widget" | "Look at how HotDogWidget.php is implemented. Follow that pattern to add a calendar widget." |

---

## Reference Context Directly

### `@` mentions for files

Instead of describing where code lives, reference it:

```
Explain what @src/auth/login.ts does
```

Claude reads the file and includes it in context. This is more token-efficient than asking Claude to search for it.

### Paste images and screenshots

Copy/paste or drag images directly into the prompt. Useful for:
- UI bugs ("fix this layout issue" + screenshot)
- Design implementation ("implement this mockup" + design image)
- Error screenshots

### Pipe data in

```bash
cat error.log | claude -p "What's causing these errors?"
```

```bash
git diff main | claude -p "Review these changes for security issues"
```

---

## Task 1: Practice Specific Prompting

Try these two approaches and compare the results:

**Approach A (vague):**
```
Tell me about the project structure
```

**Approach B (specific):**
```
Read the top-level files and list: 1) the main entry point, 2) where tests live, 3) the build command from package.json
```

Notice how Approach B gives you exactly what you need with fewer tokens consumed.

---

## Task 2: Use `@` References

Pick a file in your project and ask about it using `@`:

```
What edge cases does @src/utils/validator.ts handle?
```

Then try referencing multiple files:

```
Compare the error handling in @src/api/users.ts vs @src/api/orders.ts
```

---

## Task 3: Give Verification Criteria

Ask Claude to implement something with an explicit check:

```
Add a function to src/utils/ that converts a string to kebab-case.
Test cases: "Hello World" → "hello-world", "camelCase" → "camel-case",
"already-kebab" → "already-kebab". Write the tests and run them.
```

Watch how Claude implements, runs the tests, and fixes any failures — all without your intervention.

---

## Prompting Patterns That Save Tokens

### Point to existing patterns
```
Look at how UserService is implemented. Follow the same pattern for OrderService.
```

### Ask Claude to interview you
```
I want to build a notification system. Interview me about the requirements
before implementing anything.
```

Claude asks about edge cases, preferences, and tradeoffs you haven't considered. Better plan = fewer corrections = fewer wasted tokens.

### Scope investigations
```
# Bad — reads everything
Explain the architecture

# Good — reads what matters
How does request authentication work? Check src/middleware/auth.ts
```

---

## Key Takeaways

- **Always provide verification** — tests, expected output, screenshots
- **Be specific** — file paths, line numbers, exact scenarios
- **Use `@` references** — more efficient than describing locations
- **Scope your requests** — don't let Claude read everything

---

Continue to: [06_git_workflows.md](./06_git_workflows.md)
