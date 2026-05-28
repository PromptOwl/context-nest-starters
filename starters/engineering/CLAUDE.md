# Engineering Project Vault

This is a Context Nest vault for an engineering team. It holds the team's **decisions about the code**, not the code itself. Code lives in git; this vault holds the conventions, ADRs, runbooks, and policies that explain *why* the code looks the way it does.

## What this vault is for

You are working with an engineering team that uses this vault to:

- Document architecture decisions (ADRs) with rationale and trade-offs
- Maintain coding conventions per language so AI-generated code matches the team's standard
- Capture runbooks (deploy, rollback, incident response) so on-call doesn't reinvent them
- Hold the team's security, QA, and defect-prioritization policies in one queryable place
- Power **motions** — pre-curated packs that hydrate the right context for events like PR review, incident response, and onboarding

The vault's job is to keep AI code-generation **loyal to the team's current decisions**, not the legacy patterns in old files.

## Vault structure

- `nodes/languages/<lang>/` — per-language conventions (style, error handling, testing, deps). Five languages ship by default; trim any you don't use.
- `nodes/process/` — git flow, commit conventions, PR templates, review checklist.
- `nodes/qa/` — test strategy, coverage targets.
- `nodes/security/` — threat modeling, secrets management, dependency-vulnerability policy.
- `nodes/operations/` — deploy runbook, rollback procedure, incident response template.
- `nodes/architecture/` — ADR template, API design conventions.
- `nodes/defect-prioritization/` — severity classification, SLA matrix.
- `packs/` — three flagship motions: `pr-review`, `incident-response`, `onboarding`.

## Motions (the headline feature)

A **motion** is a pre-curated context bundle for a recurring engineering event. Each motion is a `packs/<motion>.yml` file with a saved selector. Hydrate one by running `ctx query "pack:<motion>"` or referencing it via MCP.

| Motion | When to use | What it hydrates |
|---|---|---|
| `pr-review` | Reviewing or generating a PR | Code-review checklist + language conventions for touched files |
| `incident-response` | Incident in progress | IR template + rollback decision tree + on-call rotation |
| `onboarding` | New contributor's first day | Git flow + commit conventions + ADR index + architecture overview |

## How to use Context Nest commands

```bash
ctx search <query>           # full-text search
ctx resolve <selector>       # selector query — #tag, type:X, status:X, +/-/|
ctx query "pack:<name>"      # hydrate a motion
ctx add nodes/<path>         # create a new document
ctx update <path>            # modify (auto-publishes a new version)
ctx history <path>           # version timeline
ctx validate                 # check spec conformance
ctx verify                   # verify hash chains
ctx index                    # regenerate context.yaml + INDEX.md
```

Selector syntax that works in `ctx` 0.11.x: bare `#tag`, `type:document`, `status:published`. The spec also defines `tag:#X` but the current CLI tokenizer rejects it — use the bare `#tag` form.

## First five things to do

1. **Skim the existing nodes** in each folder — they're opinionated defaults, not blanks. Read them as recommendations, not template scaffolding.
2. **Trim the languages you don't use.** Default ships Python / TypeScript / Go / Rust / Java; delete any folder for a language the team doesn't write in.
3. **Edit `nodes/process/git-flow.md` to match the team's actual branch strategy** (trunk-based vs GitHub flow vs git-flow). The default picks one; replace it with the team's truth.
4. **Add your first ADR.** Use `ctx add nodes/architecture/adrs/0001-<topic>` to record the next architecture decision the team makes. Reference `architecture/adr-template.md` for the shape.
5. **Run a motion.** Try `ctx query "pack:pr-review"` on a current PR-review session. If the resulting context isn't useful, the convention nodes need to be edited — not the pack.

## Working style

- **Be opinionated.** Engineering teams want recommendations, not "it depends." If asked "should we use TDD or BDD here?" answer with a default and the trade-offs.
- **Cite the vault.** When suggesting a pattern, say which convention node it comes from. Audit trail matters.
- **Update the convention when reality shifts.** If a team decision changes ("we're moving to trunk-based"), edit the relevant node and bump its version. The hash chain captures the migration.
- **Prefer published nodes over drafts.** AI-readable means `status: published`; drafts are intentionally hidden from query.

## Agent surfaces

This `CLAUDE.md` is mirrored to the other AI-rules files (`GEMINI.md`, `.cursorrules`, `.windsurfrules`, `.github/copilot-instructions.md`) when the vault is initialized. **The instructions are agent-agnostic** — same content reaches Cursor, Claude Code, Gemini, Windsurf, Copilot, and any MCP-speaking agent.

---
Vault powered by [Context Nest](https://github.com/PromptOwl/ContextNest) by PromptOwl — promptowl.ai
