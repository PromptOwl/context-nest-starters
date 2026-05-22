---
title: "Go dependency management"
type: document
tags: ["#convention", "#go", "#deps"]
status: published
version: 1
---

# Go dependency management

## Tooling

- **Go modules** (`go.mod` + `go.sum`). No vendoring unless a specific build environment requires it.
- `go mod tidy` runs in CI. PRs with stray imports fail.
- Module path is the repo URL: `module github.com/<org>/<repo>`.

## Pinning policy

- `go.sum` is the integrity record — committed, never gitignored.
- Top-level versions in `go.mod` are explicit (semver pinned). Indirect requires float to whatever main packages need.
- Major-version upgrades require a code change anyway (paths include `/v2`, `/v3`), so they're never silent.

## Adding a dependency

1. Check `go.mod` — duplicates and rough equivalents (gorilla/mux vs chi vs gin) are common.
2. Check the module's transitive imports. A small library that pulls in 30 others is a red flag.
3. License check: MIT / BSD / Apache 2.0 / MPL OK; GPL / AGPL flagged.
4. `go get <package>@<version>`.
5. `go mod tidy`.
6. Commit `go.mod` + `go.sum`.

## Standard library first

Go's standard library covers far more than most language ecosystems. Before adding a dependency:

- `net/http` for HTTP servers and clients.
- `log/slog` for structured logging.
- `database/sql` for SQL (use `sqlx` only if scanning into structs becomes painful).
- `crypto/*` for crypto.
- `context` for cancellation and deadlines.

Third-party libraries should add capability, not convenience.

## Vulnerability response

- `govulncheck` runs in CI on every PR. CVEs that affect actual call paths (not just imports) block the PR.
- See [[../../security/dep-vuln-policy]] for SLAs.

## Vendoring (only when required)

- `go mod vendor` if the deploy environment can't fetch modules at build time.
- `vendor/` committed when used.
- CI verifies `go.sum` and `vendor/` stay consistent (`go mod verify`).

## Antipatterns

- Replace directives in `go.mod` pointing at local paths committed to main. Use them for local dev only.
- `go get -u ./...` blanket upgrade without reviewing the diff.
- Pinning a dependency to a commit hash long-term — bump to a tagged release.

## See also

[[style-guide]] · [[../../security/dep-vuln-policy]]
