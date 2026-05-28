---
title: "Coverage targets"
type: document
tags: ["#convention", "#qa", "#coverage"]
status: published
version: 1
---

# Coverage targets

Coverage is a *floor*, not a *goal*. A repo at 80% line coverage can still ship terrible tests; a repo at 60% can have rigorous ones. The number is the bare-minimum guardrail, not a ceiling to aim at.

## Targets (defaults)

| Slice | Line | Branch |
|---|:---:|:---:|
| **New code in this PR** | ≥ 80% | ≥ 70% |
| **Whole repo (existing code)** | ratchet only — never decrease | — |
| **Critical path** (auth, payments, data integrity) | ≥ 95% | ≥ 90% |

"Critical path" is declared per repo via a tag in the test runner config or a list in the README. Default is empty; teams add files as they identify them.

## Patch coverage

PRs are evaluated on **patch coverage** — the diff alone — not on whole-repo coverage. This avoids two failure modes:

1. A small bug-fix PR getting blocked because the surrounding file is at 40%.
2. A large refactor PR pulling overall coverage up to claim "credit" without testing new code.

## Diff-aware tooling

- **Codecov** or **Coveralls** posts a PR comment with patch coverage.
- CI fails when patch coverage drops below the threshold above.
- Whole-repo coverage trend is tracked but not enforced PR-by-PR.

## What coverage *doesn't* measure

- Quality of assertions (you can hit every line and assert nothing).
- Concurrency bugs (the race detector / property tests do this).
- Performance regressions (benchmarks do this).
- Specification correctness (the test could match buggy code).

## When to ignore the floor

Coverage targets can be suspended (with a tracking ticket) when:

- **Generated code** that has its own upstream tests.
- **Vendored** code that the team will replace soon.
- **UI scaffolding** where the meaningful tests are E2E rather than unit.

These exclusions live in the coverage config file (`.coveragerc`, `nyc.config.js`, etc.) with a comment explaining the reason. Review the exclusions quarterly.

## Per-language tools

| Language | Tool |
|---|---|
| Python | pytest-cov |
| TypeScript | c8 or Vitest's built-in |
| Go | `go test -cover` + `gocover-cobertura` for CI integration |
| Rust | tarpaulin or grcov |
| Java | JaCoCo |

## See also

[[test-strategy]] · [[../testing/unit-conventions]]
