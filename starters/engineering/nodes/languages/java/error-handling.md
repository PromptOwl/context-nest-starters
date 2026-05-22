---
title: "Java error handling"
type: document
tags: ["#convention", "#java", "#error-handling"]
status: published
version: 1
---

# Java error handling

## Rules

1. **Unchecked (RuntimeException) for programmer errors.** `NullPointerException`, `IllegalArgumentException`, `IllegalStateException` — let them propagate.
2. **Checked exceptions for recoverable failures that callers should handle.** Use sparingly; checked exceptions metastasize through call chains.
3. **Domain-specific exception hierarchy.** `class BillingException extends RuntimeException`, then `class CardDeclinedException extends BillingException`.
4. **Never swallow exceptions silently.** Empty `catch` blocks are forbidden. If you legitimately can't recover, log and rethrow as a different type.
5. **Exception chaining with `cause`.** `throw new ServiceException("loading user", e)` — preserves the trace.

## try-with-resources

- Always use `try-with-resources` for `AutoCloseable` resources (streams, connections, locks).
- Don't manually `close()` in `finally` blocks. The compiler enforces the right thing.

## Boundary translation

| Boundary | Translation |
|---|---|
| REST controller | `@ExceptionHandler` → ResponseEntity with status + body |
| gRPC service | `StatusRuntimeException` with the right `Status.Code` |
| Background job | log + DLQ + alert if persistent |
| CLI | error → exit code + user-facing message |

## Logging discipline

- **SLF4J** as the facade. Don't log to System.err.
- Parameterized: `log.info("user {} created", userId)`. Not string concatenation; saves allocation when level is disabled.
- Never log secrets or PII. See [[../../security/secrets-management]] and [[../../security/pii-handling]].
- Log the exception object as the last argument: `log.error("failed loading", ex)` — SLF4J prints the chain.

## Antipatterns

- `catch (Exception e)` catching everything — narrow it.
- `throw new RuntimeException(e)` with no message — lose information.
- Logging an exception and then rethrowing — double-counts in dashboards.
- Returning `null` to mean "failed." Throw or return `Optional`.

## Standard exceptions worth using

- `IllegalArgumentException` for bad inputs from callers.
- `IllegalStateException` for object in wrong state.
- `UnsupportedOperationException` for "not implemented yet" or "feature not supported."
- `NoSuchElementException` for "looked it up, wasn't there."

## See also

[[style-guide]] · [[testing]] · [[../../operations/ir-template]]
