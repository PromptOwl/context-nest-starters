---
title: "Python error handling"
type: document
tags: ["#convention", "#python", "#error-handling"]
status: published
version: 1
---

# Python error handling

## Rules

1. **Raise specific exceptions, not `Exception`.** Define a domain hierarchy: `class BillingError(Exception)`, then `class CardDeclinedError(BillingError)`.
2. **Catch what you can handle. Re-raise what you can't.** Bare `except:` is forbidden. `except Exception:` requires a comment justifying it.
3. **Use exception chaining.** `raise NewError("context") from original_error` — preserves the trace.
4. **Don't use exceptions for flow control.** If `try/except` is faster or cleaner than `if`, you have a design problem.
5. **Errors are values at boundaries.** Inside a service, raise. At an HTTP/gRPC/event-bus boundary, translate to a status code or a typed error response.

## Logging vs raising

| Situation | Do |
|---|---|
| Unexpected condition that breaks invariants | Raise |
| Recoverable degradation (retry succeeded after one failure) | Log at WARN, don't raise |
| User input violation | Raise a typed validation error; the boundary translates to 4xx |
| External system failure | Raise; the retry/circuit-breaker layer decides |

## Logging discipline

- Use the `logging` module, not `print()`. Module-level `logger = logging.getLogger(__name__)`.
- Structured logging with `extra={}` for machine-parseable fields.
- Never log secrets, PII, or full request bodies. See [[../../security/secrets-management]] and [[../../security/pii-handling]].
- Log levels: DEBUG for dev signal, INFO for one line per request, WARN for degraded but working, ERROR for failed.

## Antipatterns

- Silently swallowing exceptions (`except Exception: pass`).
- Catching and re-raising with no added context (loses the chain).
- Using exceptions for normal control flow (StopIteration is the rare exception to this rule).
- Logging an error AND raising it — double-counts in dashboards.

## See also

[[style-guide]] · [[testing]] · [[../../operations/ir-template]]
