---
title: "ADR template"
type: document
tags: ["#convention", "#architecture", "#adr"]
status: published
version: 1
---

# ADR template

An **Architecture Decision Record (ADR)** captures one significant decision, its context, and its consequences. ADRs accumulate over time as `nodes/architecture/adrs/0001-...`, `0002-...`, etc. — numbered sequentially, never renumbered, never deleted (superseded ADRs link to their replacement).

## When to write one

- A choice between approaches that's hard to reverse.
- A decision that other engineers will encounter and need to understand.
- A technical commitment that constrains future work.
- A change that overrides an earlier ADR.

**Not** every PR is an ADR. Most code changes are tactical. ADRs are the load-bearing decisions.

## Template

```markdown
---
title: "ADR-NNNN: <Short decision name>"
type: document
tags: ["#adr", "#architecture"]
status: draft | proposed | accepted | superseded
version: 1
---

# ADR-NNNN: <Short decision name>

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Superseded by ADR-MMMM
**Authors:** @<github-username>(s)

## Context

What is the problem? What forces are at play? What constraints?
Don't list every alternative here — that goes below. Stick to the
situation that made a decision necessary.

## Decision

What did we decide? State it directly, not "we should consider".
The decision is the headline. One paragraph.

## Consequences

What follows from this decision? Both upsides and downsides.

- ✓ <good consequence>
- ✓ <good consequence>
- ✗ <bad consequence we accept>
- ✗ <bad consequence we accept>

## Alternatives considered

Briefly: what else did we look at, and why didn't we pick it?

- **<Alternative A>:** <one-line reason it lost>
- **<Alternative B>:** <one-line reason it lost>

## See also

[[<related-adr>]] · [[<convention-this-decision-implements>]]
```

## Lifecycle

- **Draft:** circulating for feedback before commitment.
- **Proposed:** stable enough for review; can still change.
- **Accepted:** committed. Code can rely on this.
- **Superseded:** replaced by a later ADR. Don't delete — leave it in place with a link forward.

## What makes a good ADR

- **Short.** One page is the target; two if the trade-off is genuinely complex.
- **Honest about the downsides.** "We chose X; the downsides are Y and Z" beats "X is great." Future readers need to understand the cost.
- **One decision per ADR.** Don't bundle.
- **Reference the artifacts.** Link to RFC threads, benchmark results, the PR that implemented it.

## What makes a bad ADR

- Vague: "we'll consider performance implications." Decide.
- One-sided: only the upsides; no acknowledgment of cost.
- Stale: written before code existed, never updated when reality diverged.
- Tactical: "Use Tailwind for this new component" — not architectural, just preference.

## See also

[[service-design-checklist]] · [[api-design]]
