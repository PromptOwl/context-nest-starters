---
title: "Commit message conventions"
type: document
tags: ["#convention", "#process", "#git"]
status: published
version: 1
---

# Commit message conventions

This starter ships with **Conventional Commits** as the default. Adopt or replace per team.

## Format

```
<type>(<scope>): <short summary>

<body — what and why; not how>

<footer — refs, breaking-change notes>
```

## Types

- `feat:` — a new feature
- `fix:` — a bug fix
- `refactor:` — code change that neither fixes a bug nor adds a feature
- `perf:` — performance improvement
- `test:` — adding or correcting tests
- `docs:` — documentation only
- `chore:` — tooling, build, CI changes
- `style:` — formatting, no code change
- `revert:` — reverting a previous commit

## Examples

```
feat(auth): add SSO via SAML

Supports both IdP-initiated and SP-initiated flows. Test users are
provisioned via JIT — no manual setup required for new domains.

Refs: TKT-1234
```

```
fix(payments): retry idempotent charges on transient gateway errors

Was: a 503 from the gateway surfaced as a failed charge. The charge
was actually queued upstream; retry would have succeeded.

Refs: TKT-1567
```

## Squash-merge semantics

When the team uses squash-merge (recommended — see [[git-flow]]), only the PR title + body becomes the commit message. Make those Conventional-Commits-shaped.

## Footers worth using

- `Refs:` — work-tracker ticket(s)
- `Co-Authored-By:` — pair programmer or AI assistant credit
- `BREAKING CHANGE:` — start a new paragraph; describes incompat
- `Fixes:` — issue tracker reference that closes on merge

## Anti-patterns

- "Update file.ts" — type, scope, summary all missing
- "WIP" — never. Use draft PRs instead
- "fixed" — fixed what? Multi-line message with the actual diagnosis
- AI-generated marketing copy ("This commit elegantly handles...") — short, factual

## Tooling

- **commitlint** in CI enforces the convention.
- **commitizen** offers an interactive prompt for authors who want help formatting.
- The CHANGELOG can be generated from Conventional Commits (`standard-version`, `release-please`).

## See also

[[git-flow]] · [[pr-template]]
