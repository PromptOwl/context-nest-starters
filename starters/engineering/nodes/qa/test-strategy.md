---
title: "Test strategy"
type: document
tags: ["#convention", "#qa", "#testing"]
status: published
version: 1
---

# Test strategy

## Test pyramid (the default)

```
       /\
      /E2\      few, slow, brittle — only the critical user journeys
     /----\
    /  I   \    moderate, real dependencies, integration boundary
   /--------\
  /    U     \  many, fast, no I/O — most logic verified here
 /------------\
```

- **Unit (~70% of test count):** pure functions, isolated objects. No I/O. Fast (< 100ms per test).
- **Integration (~25%):** module boundaries with real dependencies (DB, queue, file system). Slower; run on PR.
- **E2E (~5%):** smoke set runs on PR; full set runs nightly. Covers the user journeys that *must* keep working.

## What goes where

| Behavior | Unit | Integration | E2E |
|---|:---:|:---:|:---:|
| Pure-function logic | ✓ | | |
| Class / module contract | ✓ | | |
| Database query correctness | | ✓ | |
| HTTP handler request/response shape | | ✓ | |
| Cross-service flow (auth → API → storage) | | ✓ | |
| User-visible business outcome | | | ✓ |

## TDD or BDD?

- This starter doesn't mandate either. Pick per team and edit this node.
- **TDD** is the default for greenfield work and bug fixes. Write the failing test, make it pass, refactor.
- **BDD** (Cucumber, Behave) shines when product / QA writes specs alongside engineering. Adopt if cross-functional spec authorship is a real workflow, not just an aspiration.

## When to break the pyramid

- **Infrastructure-heavy systems** (data pipelines, scientific compute) flip the ratio. Integration may be 60% with E2E and unit balanced.
- **API contracts you don't own** are usefully covered by contract tests (Pact, OpenAPI schemas).
- **Performance-sensitive code** gets benchmarks (`go test -bench`, `pytest-benchmark`, Criterion).

## What NOT to test

- Code that's already covered by a well-tested library's tests.
- Configuration files (the deploy pipeline tests these).
- Trivial getters/setters with no logic.
- Code paths that exist only to satisfy a tool (type-checker dummies).

## Flaky tests

- A flaky test is a bug. Don't retry; diagnose.
- Common causes: timing assumptions, test order dependencies, real network calls in unit tests.
- Three strikes rule: if a test has failed flakily three times in a quarter, it's deleted or rewritten. Documented in [[../operations/ir-template]] retros when relevant.

## See also

[[coverage-targets]] · [[../testing/unit-conventions]] · [[../testing/integration-conventions]]
