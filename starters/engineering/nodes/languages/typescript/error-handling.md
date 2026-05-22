---
title: "TypeScript error handling"
type: document
tags: ["#convention", "#typescript", "#error-handling"]
status: published
version: 1
---

# TypeScript error handling

## Rules

1. **Result types or thrown errors — pick one per module, don't mix.** Recommended: throw for internal failures, return `Result<T, E>` at module boundaries where the caller is expected to inspect.
2. **`Error` subclasses for domain failures.** `class PaymentDeclinedError extends Error` with a discriminator field.
3. **Never throw strings or plain objects.** `throw new Error("...")` not `throw "..."`. Stack traces and `instanceof` checks depend on this.
4. **`unknown` in catch.** `catch (err: unknown)` — narrow with `instanceof` before using.
5. **Async errors propagate via promise rejection.** Don't double-handle: either `try/catch` or `.catch()`, not both around the same await.

## Boundary translation

| Boundary | Translation |
|---|---|
| HTTP handler | Caught error → status code + structured body |
| MCP tool | Caught error → tool-error response |
| Background job | Caught error → log + dead-letter queue |
| UI event handler | Caught error → toast + Sentry breadcrumb |

## Logging discipline

- One logger module, one log format (JSON in production, pretty in dev).
- Levels: `debug` / `info` / `warn` / `error`. No custom levels.
- Never log secrets, tokens, or PII. See [[../../security/secrets-management]] and [[../../security/pii-handling]].
- `error` log includes the full chain (use `err.cause`).

## Antipatterns

- `catch (e) { console.error(e) }` and continuing — silent failure.
- `Promise.all` without considering whether one rejection should cancel the others. Use `Promise.allSettled` when you want all to run.
- Throwing inside event emitters — listeners can't catch.
- `as Error` casts in catch blocks. Use `instanceof Error` and a fallback.

## See also

[[style-guide]] · [[testing]] · [[../../operations/ir-template]]
