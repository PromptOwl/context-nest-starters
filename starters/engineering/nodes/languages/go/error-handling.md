---
title: "Go error handling"
type: document
tags: ["#convention", "#go", "#error-handling"]
status: published
version: 1
---

# Go error handling

## Rules

1. **Return errors, don't panic.** Panic is reserved for programmer errors (nil deref, impossible state). Recoverable failure is `error`.
2. **Wrap with context.** `fmt.Errorf("loading config: %w", err)` — preserves the chain via `errors.Is` / `errors.As`.
3. **Check every error.** `errcheck` enforces this. Explicit ignore (`_ = thing()`) requires a comment justifying it.
4. **Sentinel errors for known conditions.** `var ErrNotFound = errors.New("not found")`. Callers check with `errors.Is`.
5. **Typed errors for structured detail.** `type ValidationError struct { Field string }` — callers extract with `errors.As`.

## Patterns

```go
// Wrapping
if err := db.Get(id, &user); err != nil {
    return fmt.Errorf("user %s: %w", id, err)
}

// Sentinel check
if errors.Is(err, sql.ErrNoRows) {
    return ErrNotFound
}

// Typed extraction
var verr *ValidationError
if errors.As(err, &verr) {
    return badRequest(verr.Field)
}
```

## panic / recover

- `panic` only on programmer errors that should crash the process.
- `recover` only at goroutine top-level boundaries (HTTP handlers, scheduled jobs) — and immediately convert to logged error.
- Library code never recovers; let the caller decide.

## Logging discipline

- Use `slog` (stdlib structured logger). Same fields per request.
- Never log secrets or PII. See [[../../security/secrets-management]] and [[../../security/pii-handling]].
- Log at the boundary where the error was handled, not at every wrap site.

## Boundary translation

| Boundary | Translation |
|---|---|
| HTTP handler | error → status code (use a small mapper) |
| gRPC server | error → `codes.X` status |
| Background worker | error → log + retry decision |
| CLI | error → exit code + user-facing message |

## Antipatterns

- `if err != nil { return err }` with no wrap loses the trace.
- `panic` for "should never happen" — log and return instead so prod observes it.
- Comparing error strings: `err.Error() == "not found"` is brittle. Use sentinels.

## See also

[[style-guide]] · [[testing]] · [[../../operations/ir-template]]
