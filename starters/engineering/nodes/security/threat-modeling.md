---
title: "Threat modeling"
type: document
tags: ["#convention", "#security", "#threat-modeling"]
status: published
version: 1
---

# Threat modeling

A threat model is **not** a security audit. It's a tool engineers use during design to figure out what could go wrong, before code exists to defend.

## When to do one

- **New service or feature** that handles user data, payments, auth, or any cross-boundary trust.
- **Architectural change** that introduces a new trust boundary (new external integration, new data store, new permission model).
- **Pre-launch checkpoint** before exposing a system to a new population (internal → external, free → paid, region expansion).

For day-to-day features that don't move trust boundaries, the existing model covers you.

## STRIDE — six categories worth memorizing

| Letter | Threat | Mitigation example |
|---|---|---|
| **S**poofing | Pretending to be someone else | Strong auth, MFA, signed requests |
| **T**ampering | Modifying data in flight or at rest | TLS, signed messages, immutable audit logs |
| **R**epudiation | Denying you did something | Audit logging, signed user actions |
| **I**nformation disclosure | Leaking data | Encryption, access control, [[pii-handling]] |
| **D**enial of service | Exhausting resources | Rate limits, quotas, circuit breakers |
| **E**levation of privilege | Doing things you shouldn't | Least-privilege auth, defense in depth |

## The 30-minute model

For most features, this is sufficient:

1. **Draw the data flow.** Boxes for components, arrows for data. Mark trust boundaries (network, process, user).
2. **For each crossing**, walk STRIDE. Most of the time only 2–3 categories apply.
3. **For each applicable threat**, decide: mitigate, accept, transfer, or avoid.
4. **Record decisions as ADRs.** Don't lose the reasoning.

## Outputs

- A diagram (whatever tool — Excalidraw, draw.io, Mermaid).
- A table: threat → category → decision → owner.
- An ADR or set of ADRs in [[../architecture]] capturing the design choices.
- A linked test for each accepted threat that proves the mitigation works.

## What NOT to do

- Don't threat-model for every PR. Most changes don't move trust boundaries.
- Don't outsource it to a "security team review" at the end. The model belongs to whoever's designing the system.
- Don't write the threat model in a document nobody reads. Keep it short; commit it to the repo or this vault.

## See also

[[secrets-management]] · [[auth-patterns]] · [[../architecture/adr-template]]
