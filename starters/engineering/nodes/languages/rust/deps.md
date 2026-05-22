---
title: "Rust dependency management"
type: document
tags: ["#convention", "#rust", "#deps"]
status: published
version: 1
---

# Rust dependency management

## Tooling

- **Cargo.** No alternatives.
- `Cargo.lock` committed for binaries, library `Cargo.lock` is committed too (current best practice).
- `cargo update` is intentional; not automatic.

## Pinning policy

- Top-level versions in `Cargo.toml` use caret (`"1.2"` = `^1.2`). Minor and patch float; major requires explicit bump.
- `Cargo.lock` records exact resolved versions.
- For reproducible builds in CI, `cargo build --locked` fails if lockfile would change.

## Adding a dependency

1. Search `Cargo.toml` — overlap is common (e.g., `reqwest` vs `ureq` vs `hyper`).
2. Look at the feature flags. Many crates default to large features; disable with `default-features = false` and re-enable only what's needed.
3. Check compile time impact. `cargo bloat` and `cargo tree --duplicates` surface trouble.
4. License (MIT / Apache 2.0 OK; copyleft requires legal review).
5. `cargo add <crate>` (or `cargo add --dev <crate>`).
6. `cargo build` to confirm the resolution.
7. Commit `Cargo.toml` + `Cargo.lock`.

## Feature flags

- Crates expose features via `[features]` in `Cargo.toml`. Default features can be big.
- Always declare what features your crate exposes — don't rely on transitive features being available.
- Test with `--no-default-features` periodically to catch implicit assumptions.

## Workspaces

- Multi-crate repos use `[workspace]` in the root `Cargo.toml`.
- Workspace `[workspace.dependencies]` deduplicates versions across member crates.
- Internal crates use `{ workspace = true }` for shared deps.

## Vulnerability response

- `cargo audit` (from the cargo-audit subcommand) runs in CI on every PR. Advisories block.
- `cargo deny` for license and supply-chain rules. Maintain `deny.toml` in repo root.
- See [[../../security/dep-vuln-policy]] for SLAs.

## Antipatterns

- `path = "..."` dependencies pointing outside the workspace, committed to main.
- `git = "..."` pointing at a branch — pin to a tagged release or fork into a maintained crate.
- Pulling in `tokio` "full" features when you only need `rt-multi-thread`.

## See also

[[style-guide]] · [[../../security/dep-vuln-policy]]
