---
title: "On-call rotation"
type: document
tags: ["#runbook", "#operations", "#oncall"]
status: published
version: 1
---

# On-call rotation

## Schedule

- Weekly rotation, handoff at the start of the business day on a fixed weekday (Monday is most common; pick what suits time zones).
- Primary + secondary always. Secondary takes over if primary is unreachable for > 15 minutes.
- Follow-the-sun if multiple time zones: each region covers its own business hours; alerts in off-hours route to the active region.

## Eligibility

- Anyone who has completed the on-call onboarding can be primary.
- Onboarding includes: shadow shift, deploy + rollback practice, IR drill participation, runbook walkthrough.
- Backup-only for the first month — never carry the pager solo until shadow shifts are complete.

## Handoff

The outgoing on-call sends a written handoff in `#oncall` (or equivalent) at the end of their shift. Covers:

- Open incidents (status, IC if active).
- Anything that pinged that didn't escalate but the next person should know about.
- Outstanding alerts on Pause / Silence (with expiry).
- Deploys planned for the upcoming shift.

Don't end your shift until the handoff is acknowledged.

## What on-call covers

- Production alerts (PagerDuty / Opsgenie / equivalent).
- Customer-reported outages (escalated from support).
- Deploy assistance for engineers shipping risky changes.
- After-hours security alerts.

## What on-call does NOT cover

- Project work (unless they want to fill gaps between alerts).
- Customer support tickets that aren't outages.
- Acceptance / approval of routine PRs.

## Page criteria

A page wakes someone up at 3 AM. Only page for things worth waking someone for:

- SEV-0 / SEV-1 (see [[ir-template]]).
- Sustained error-rate or latency regressions (not single spikes).
- Security alerts that require immediate human judgment.

Everything else: open a ticket. The on-call reviews tickets in the morning.

## Compensation and recovery

- Time-in-lieu for any active page outside business hours.
- A page that triggers > 2 hours of work: full next-day off, no questions.
- A "burned-down" shift (multiple major incidents): swap with the next-week on-call.

## Anti-patterns

- "Hero" on-call who refuses to escalate. Burnout follows.
- Page volume that never decreases. The on-call should be filing tickets to reduce alerts that don't matter, not stoically absorbing noise.
- Rotation that bypasses junior engineers. They never learn the system.
- Coverage gaps over holidays. Schedule them explicitly.

## See also

[[ir-template]] · [[rollback]] · [[deploy-runbook]]
