---
title: "TypeScript testing conventions"
type: document
tags: ["#convention", "#typescript", "#testing"]
status: published
version: 1
---

# TypeScript testing conventions

## Stack

- **Vitest** for unit tests. Fast, native ESM, Jest-compatible API.
- **Playwright** for E2E. Not Cypress (slower CI, browser-restricted).
- **@testing-library/react** for component tests when applicable.
- **msw** for HTTP mocking at the protocol layer.

## Structure

- Co-located: `src/foo/bar.ts` ↔ `src/foo/bar.test.ts`. Easier to find, easier to delete with the unit.
- Test files end in `.test.ts` or `.spec.ts` consistently per repo.
- Setup in `vitest.setup.ts`; one file, no per-folder overrides.

## Naming and shape

```ts
describe("charge", () => {
  it("succeeds when card is valid", () => {
    // arrange / act / assert
  });
});
```

`it("succeeds when card is valid")` reads as a sentence. `it("should charge correctly")` does not.

## What to test

- **Pure functions:** every meaningful branch.
- **State machines:** every transition.
- **API handlers:** happy path + every documented error.
- **UI components:** behavior, not implementation. "Click the button → confirm modal opens" — not "useState was called with x."

## Mocking discipline

- Mock at module boundaries (`vi.mock("./api")`) or via dependency injection.
- Avoid mocking what you own deeply — your test ends up asserting your mock, not your code.
- `msw` is preferred over `vi.mock` for HTTP because it tests at the protocol surface.

## E2E

- Run on every PR against the main branch.
- Smoke set runs in < 5 minutes; full set runs nightly.
- Flaky tests are deleted, not retried. See `flaky-test-policy` (add when needed).

## Coverage target

80% on new code; see [[../../qa/coverage-targets]].

## See also

[[error-handling]] · [[../../qa/test-strategy]] · [[../../testing/integration-conventions]]
