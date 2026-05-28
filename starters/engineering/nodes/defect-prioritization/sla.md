---
title: "SLA matrix"
type: document
tags: ["#convention", "#defect-prioritization", "#sla"]
status: published
version: 1
---

# SLA matrix

What we commit to, by severity. Edit per team — these are the defaults, not gospel.

## Response and resolution targets

| Severity | First response | Status update cadence | Target resolution |
|---|---|---|---|
| **SEV-0** | 5 minutes | Every 15 min | Mitigation < 1 hour; root cause within 24 hours |
| **SEV-1** | 30 minutes | Every 30 min | Mitigation < 4 hours; root cause within 3 business days |
| **SEV-2** | 4 business hours | Daily | Within the current sprint |
| **SEV-3** | 2 business days | Weekly while open | Next normal sprint or two |

"Response" = an engineer has acknowledged and is investigating. "Resolution" = the issue is fixed or has a permanent workaround in production.

## Mitigation vs root cause

- **Mitigation:** users stop being broken. Roll back, toggle flag, route around. Can be a band-aid.
- **Root cause:** the underlying defect is fixed. Often follows mitigation by days.

For SEV-0/1, mitigation SLA is what matters to customers. Root cause SLA is what matters to engineering hygiene. Both have targets.

## Off-hours

- SEV-0 / SEV-1: SLAs apply 24/7 (on-call covers).
- SEV-2 / SEV-3: business hours only.

If an off-hours SEV-2 sits in the queue overnight, that's expected. If an off-hours SEV-1 sits in the queue, on-call missed it — investigate.

## SLA breaches

When the SLA is breached, the next step isn't punishment. It's a discussion at the next on-call retro:

- Was the severity classification correct?
- Was the on-call structurally capable of responding (page received, runbook available)?
- Was anything in the way (no access, unclear ownership)?

Three breaches in a quarter = the SLA / staffing model needs adjustment.

## What's NOT covered by these SLAs

- Feature requests. Different process.
- Optimization work. Roadmap, not SLAs.
- Speculative concerns ("this might break under 10x load"). File research tickets.

## Customer-visible SLAs

If the product publishes external SLAs (uptime guarantees, paid-tier response promises), those are separate from this internal matrix and usually stricter. The internal matrix is the *minimum*; external commitments override upward.

## See also

[[severity-classification]] · [[../operations/oncall-rotation]]
