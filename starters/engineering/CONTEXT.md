---
title: "Engineering Vault"
---

# Engineering Vault

This vault holds the team's **decisions about the code**, not the code itself. Code lives in git; this vault holds the conventions, ADRs, runbooks, and policies that explain *why* the code looks the way it does — so the team's AI agents can apply the team's current standards rather than the patterns they happen to see in old files.

## Layout

```
nodes/
  languages/        per-language conventions (Python, TS, Go, Rust, Java)
  process/          git flow, PR template, commit conventions, review checklist
  qa/               test strategy, coverage targets
  security/         threat modeling, secrets, dep-vuln policy
  operations/       deploy runbook, rollback, IR template, on-call rotation
  architecture/     ADR template, API design, service design checklist
  defect-prioritization/  severity classification, SLA matrix
packs/              motions — pre-curated selectors for common events
```

## Motions

A **motion** is a context bundle for a recurring engineering event. Hydrate one to give the agent the right slice of the vault for the moment:

- `pr-review` — reviewing or generating a PR
- `incident-response` — coordinating an active incident
- `onboarding` — new contributor's first session

Run with `ctx query "pack:<motion>"` or reference via MCP.

## How to use this vault

1. **Read it as recommendations, not blanks.** Every node has opinionated default content. Edit to match the team's actual practice.
2. **Trim what you don't use.** Five languages ship by default — delete folders for languages the team doesn't write in.
3. **Add ADRs as you go.** `nodes/architecture/adrs/0001-...` numbered, never renumbered, never deleted.
4. **The vault is queryable, not static.** `ctx resolve "#convention + #python"` returns every Python convention. The agent can compose context this way.

## Operating instructions for AI agents

See `CLAUDE.md` (mirrored to `GEMINI.md`, `.cursorrules`, `.windsurfrules`, `.github/copilot-instructions.md`). Same content across surfaces — the vault is agent-agnostic by design.

## Citing the vault

When the agent applies a convention, it should say so: *"Following `nodes/languages/python/error-handling.md` v1: I'm wrapping the gateway error with context before raising."* That's the audit trail the product invariant depends on.
