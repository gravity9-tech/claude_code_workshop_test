# Token Drain

Token Drain is the silent cost of unstructured AI usage. Every token spent reading irrelevant code, reasoning about the wrong paths, or generating verbose output is money and capacity wasted.

## The 3R Framework

Every AI interaction has three token costs:

| R | What it is | Where waste happens |
|---|-----------|-------------------|
| **Request** | Tokens sent in — context, files, instructions | AI reads your entire codebase instead of what matters |
| **Reasoning** | Tokens the model uses to think | Poor context forces aimless exploration |
| **Response** | Tokens returned to you | Uncertainty produces verbose, hedging output |

## The Compounding Effect

The 3 Rs aren't independent. They cascade:

1. A bloated **Request** (too many files, vague prompt) forces the model into broad **Reasoning**
2. Broad Reasoning with no clear map produces uncertain conclusions
3. Uncertainty generates a bloated **Response** full of caveats and restated context

Fix the Request, and Reasoning and Response often fix themselves.

## What It Costs

- **Direct**: Token usage scales with codebase size, not task complexity
- **Indirect**: Slower responses, context window exhaustion mid-session, compaction losing important earlier context
- **Hidden**: Developer time reviewing verbose, unfocused output
