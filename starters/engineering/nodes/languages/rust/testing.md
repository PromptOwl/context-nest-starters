---
title: "Rust testing conventions"
type: document
tags: ["#convention", "#rust", "#testing"]
status: published
version: 1
---

# Rust testing conventions

## Stack

- **Standard library `#[test]`** runs everything. Use `cargo test` and `cargo nextest` (faster on big workspaces).
- **proptest** or **quickcheck** for property-based tests where invariants are the contract.
- **insta** for snapshot tests.
- **mockall** for trait mocking when the seam is genuine.
- **testcontainers-rs** for integration against real services.

## Structure

- Unit tests: bottom of the module file in `#[cfg(test)] mod tests { ... }`.
- Integration tests: `tests/` directory. Each `tests/<name>.rs` is its own crate that only sees the public API.
- Doc tests: examples in `///` comments. They run with `cargo test --doc`.

## Naming and shape

```rust
#[test]
fn charge_succeeds_when_card_valid() {
    // arrange / act / assert
}
```

Test names are sentences. No `_test` suffix on already-`#[test]`-marked functions.

## Patterns

- Use `assert_eq!` / `assert_ne!` with diagnostic messages: `assert_eq!(got, want, "input was {input}")`.
- `#[should_panic(expected = "...")]` for testing panic paths.
- `Result<(), TestError>` returning tests for `?`-propagating setup.

## Property-based testing

```rust
proptest! {
    #[test]
    fn parse_roundtrips(input in ".*") {
        let parsed = parse(&input)?;
        let serialized = serialize(&parsed);
        prop_assert_eq!(input, serialized);
    }
}
```

Use this for serializers, parsers, anything with a stated invariant.

## What to test

- Public API behavior.
- Error paths (each variant of the error enum should appear in a test).
- Boundary cases (empty, max, unicode, integer limits).

## What not to test

- Private implementation details — refactor the structure if you need to.
- Code paths that exist only to please the borrow checker.

## See also

[[error-handling]] · [[ownership]] · [[../../qa/test-strategy]]
