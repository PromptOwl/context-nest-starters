---
title: "Rust style guide"
type: document
tags: ["#convention", "#rust", "#style"]
status: published
version: 1
---

# Rust style guide

## Toolchain

- `rustfmt` for formatting. Default style. Run via `cargo fmt --all`.
- `clippy` for linting. CI runs `cargo clippy --all-targets -- -D warnings` — warnings fail.
- Pin the Rust toolchain with `rust-toolchain.toml`; bump quarterly.
- MSRV (Minimum Supported Rust Version) declared in `Cargo.toml` `rust-version`.

## Naming

- `snake_case` for functions, methods, variables, modules, crates.
- `PascalCase` for types, traits, enum variants.
- `SCREAMING_SNAKE_CASE` for constants and statics.
- Lifetimes lowercased (`'a`, `'static`). Use longer names (`'src`) when meaning matters.

## Module layout

- Use module-per-file (`module.rs`) over the older `module/mod.rs` form.
- Tests live in `mod tests` blocks at the bottom of the file (unit) or `tests/` directory (integration).
- Public surface in `lib.rs` or `mod.rs` re-exports; everything else `pub(crate)`.

## Idioms

- **`Result<T, E>` not panic for recoverable errors.** Panic is for invariants.
- **`?` operator for error propagation.** No bare `unwrap()` or `expect()` outside tests and main.
- **Iterators over manual loops** where the iterator is clearer.
- **Borrow checker is your friend.** Reach for `Clone` only when ownership genuinely can't be split.
- Prefer `&str` over `String` in function arguments unless ownership is needed.
- Don't reach for `Rc`/`Arc` early — restructure to single-ownership first.

## Visibility

- `pub` is opt-in. Default to private; add `pub` only when crossing a module boundary.
- `pub(crate)` for items shared within the crate but not exposed to consumers.
- `pub(super)` for sibling-module use.

## Comments

- Doc comments (`///`) on every public item.
- Module-level docs (`//!`) at the top of `lib.rs`.
- Examples in doc comments run as tests with `cargo test --doc`.

## See also

[[error-handling]] · [[testing]] · [[ownership]] · [[deps]]
