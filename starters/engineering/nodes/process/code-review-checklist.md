---
title: "Code review checklist"
type: document
tags: ["#convention", "#process", "#code-review"]
status: published
version: 1
---

# Code review checklist

The reviewer's job is not to find typos. It's to verify the change is correct, maintainable, and doesn't introduce risk the author didn't anticipate.

## Correctness
- [ ] The change does what the PR description says it does.
- [ ] Edge cases are handled (empty, null, max-size, concurrent access).
- [ ] Error paths are tested. Not just the happy path.
- [ ] Tests would have caught the bug the PR claims to fix.
- [ ] No regression in test coverage.

## Design
- [ ] The change fits the existing structure. Big departures are flagged as new ADRs.
- [ ] Abstractions are appropriate — not over-engineered, not under-abstracted.
- [ ] Public APIs are minimal. Internal stays internal.
- [ ] No premature optimization. Optimize when there's a measured problem.

## Security
- [ ] No secrets in code, no secrets in tests, no secrets in logs.
- [ ] User input is validated at the boundary.
- [ ] SQL is parameterized; no string concatenation.
- [ ] Authentication and authorization are not bypassed for "convenience."
- [ ] No new dependency added without checking license + vulnerabilities.

## Reliability
- [ ] No silent error swallowing.
- [ ] Logs are structured and useful for incident response.
- [ ] No new infinite loops, no unbounded queues, no unconstrained retries.
- [ ] Timeouts and circuit breakers are in place for external calls.

## Style
- [ ] Formatter and linter pass (these should be enforced in CI; don't waste review on style).
- [ ] Names are clear and consistent with surrounding code.
- [ ] Comments explain *why*, not *what*.
- [ ] No commented-out code, no `console.log` / `print` debug statements left in.

## Documentation
- [ ] README updated if user-facing behavior changed.
- [ ] CHANGELOG updated.
- [ ] Public API changes have updated docs / type signatures.

## What NOT to do as a reviewer

- Don't request changes for stylistic preferences not enforced by the linter.
- Don't ask the author to refactor adjacent code "while you're in there." Separate PR.
- Don't approve based on the PR title. Read the diff.

## See also

[[pr-template]] · [[../languages/python/style-guide]] · [[../languages/typescript/style-guide]]
