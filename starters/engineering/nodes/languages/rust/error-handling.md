---
title: "Rust error handling"
type: document
tags: ["#convention", "#rust", "#error-handling"]
status: published
version: 1
---

# Rust error handling

## Rules

1. **Recoverable failures are `Result<T, E>`.** Panic is reserved for invariants and unrecoverable programmer errors.
2. **Libraries define typed errors.** Use `thiserror` for ergonomic `Error` derivation.
3. **Applications can use `anyhow`.** Trades typed errors for ergonomics at the binary's outer layer.
4. **No `unwrap()` / `expect()` outside `main` or tests.** Replace with `?` and propagate.
5. **`From` impls handle conversion at module boundaries.** Don't litter call sites with `.map_err`.

## thiserror pattern (libraries)

```rust
#[derive(Debug, thiserror::Error)]
pub enum BillingError {
    #[error("card declined: {reason}")]
    CardDeclined { reason: String },
    #[error("payment provider unreachable")]
    ProviderDown(#[from] reqwest::Error),
    #[error("internal: {0}")]
    Internal(String),
}
```

## anyhow pattern (binaries)

```rust
use anyhow::{Context, Result};

fn run() -> Result<()> {
    let config = load_config().context("loading config")?;
    let server = Server::new(config).context("initializing server")?;
    server.serve().context("serving")
}
```

## panic policy

- `panic!` only on invariants the type system can't express (e.g. integer overflow on counters that proved bounded by external invariant).
- `assert!` and `debug_assert!` are panics with a message — same rule.
- `unwrap()` / `expect()` in `main` or `#[test]` is OK; in library code it isn't.
- `unwrap_or`, `unwrap_or_else`, `unwrap_or_default` for fallback values are fine — they don't panic.

## Logging

- `tracing` for structured logging in async code. `log` is acceptable in simpler crates.
- Never log secrets or PII. See [[../../security/secrets-management]] and [[../../security/pii-handling]].
- `error!` includes the full source chain via `tracing::error!(error = %err, ...)`.

## Antipatterns

- `unwrap()` "because the value can't be None here" — add the type-level proof instead, or use `expect("invariant: …")` with a real comment.
- `Box<dyn Error>` everywhere — typed errors carry more information.
- `Result<T, String>` — strings aren't errors. Use a real type.

## See also

[[style-guide]] · [[testing]] · [[../../operations/ir-template]]
