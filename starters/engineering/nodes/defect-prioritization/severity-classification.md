---
title: "Severity classification"
type: document
tags: ["#convention", "#defect-prioritization", "#severity"]
status: published
version: 1
---

# Severity classification

Severity describes the **impact on users / business**. Priority describes **when we'll fix it**. They're related but not the same; severity is observed, priority is decided.

## Severity levels

### SEV-0 — Critical
- **Impact:** All or most users blocked. Data loss. Security breach. Money at stake.
- **Examples:** Login is down. Database returning corrupt data. Credentials leaked. Charges going through twice.
- **Response:** Drop everything. Page on-call. See [[../operations/ir-template]].
- **Communication:** Status page red within 5 minutes. External comms within 30.

### SEV-1 — Major
- **Impact:** Major feature degraded. Significant user population affected. No good workaround.
- **Examples:** Checkout failing for 10% of customers. Search returning wrong results. Notifications stopped.
- **Response:** On-call + co-pilot. Senior engineer aware.
- **Communication:** Status page yellow. Update every 30 minutes.

### SEV-2 — Moderate
- **Impact:** Localized degradation. Some users affected. Workaround exists.
- **Examples:** Specific report failing for one customer. Slowness for one region. Email digest delayed.
- **Response:** Investigate during business hours. No paging.
- **Communication:** Internal channel only unless customer-facing.

### SEV-3 — Minor
- **Impact:** Cosmetic or low-impact issue. Workaround obvious. No user data risk.
- **Examples:** UI alignment bug. Confusing error message. Slow-but-functional admin tool.
- **Response:** Ticket; fix in next normal sprint.
- **Communication:** None required.

## Promotion / demotion

Severity isn't fixed at filing. Update as the picture changes:

- A SEV-2 affecting one customer becomes SEV-1 if you discover it affects 30% of users.
- A SEV-1 demotes to SEV-2 if a quick workaround is deployed and pressure relieves.
- The IC owns the current severity during an incident. Document changes in the channel timeline.

## What severity is NOT

- It's not a priority — see [[sla]] for that.
- It's not a punishment for the engineer who introduced the bug.
- It's not a measure of how hard the fix is. A SEV-0 caused by a one-line config typo is still SEV-0.

## Anti-patterns

- Filing every bug as SEV-2 to avoid the on-call rotation. Compresses the spectrum into uselessness.
- Filing every bug as SEV-1 to get attention. Cries wolf.
- Severity inflation during incidents because it feels urgent. The criteria above are the criteria.
- Closing a SEV-0 ticket without a post-incident retrospective.

## See also

[[sla]] · [[../operations/ir-template]]
