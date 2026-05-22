---
title: "Incident response template"
type: document
tags: ["#runbook", "#operations", "#incident"]
status: published
version: 1
---

# Incident response template

## Roles (assigned in the first 5 minutes)

- **Incident commander (IC).** Coordinates. Not the person typing fixes. Makes go/no-go calls. Communicates externally.
- **Operator(s).** Hands on keyboard. Reports findings to IC. Doesn't make policy calls without IC's sign-off.
- **Scribe.** Timestamps every event in the incident channel. Saves the IC from doing timeline reconstruction later.
- **Comms.** External-facing updates (status page, customer support). Often the IC for small incidents; separated for big ones.

## Severity (declared in the first 60 seconds)

| Sev | Definition | Examples | Response |
|---|---|---|---|
| **SEV-0** | Full outage; data loss; security breach | All users can't log in; database corrupted | Drop everything. Page everyone. Status-page red. |
| **SEV-1** | Major degradation | Checkout failing 10% of attempts | On-call + co-pilot + lead engineer; status-page yellow |
| **SEV-2** | Localized degradation | One feature broken for one customer | On-call investigates; no broad notification |
| **SEV-3** | Minor degradation, has workaround | Slow but functional | Ticket; fix in next sprint |

See [[../defect-prioritization/severity-classification]] for the longer definitions.

## During the incident

1. **Open a dedicated channel.** `#inc-YYYY-MM-DD-<short-name>`. Pin: severity, IC, current status.
2. **Status page.** Update within 5 minutes of declaring SEV-0/1. Even if all you can say is "we're investigating."
3. **Mitigate first; root-cause later.** Roll back the deploy. Failover. Drain traffic. Buy time to think.
4. **Timestamped notes.** Scribe writes everything: every command run, every hypothesis tested, every observation.
5. **No silent acks.** When someone joins the channel, IC says what role they're picking up.

## Mitigation playbook (in order)

1. Roll back recent deploys. See [[rollback]].
2. Toggle feature flags off.
3. Drain affected hosts / failover to standby.
4. Throttle / rate-limit to protect the rest of the system.
5. Scale up (sometimes the issue is load).

## Closing the incident

- Status page green.
- Channel pin updated: "RESOLVED."
- Comms posts final external update.
- Scribe's timeline is preserved (archive the channel, screenshot the pins).
- Schedule the post-incident review within 48 hours.

## Post-incident review

- **Blameless.** Failures are system signals, not personal failures. The system that let an individual mistake cause the incident is the thing to fix.
- **Root cause is plural.** Almost no incident has one cause. Identify the chain.
- **Action items have owners and dates.** "Improve monitoring" is not an action item. "Add latency alert on /checkout with threshold 500ms, owner: Alice, by 2026-06-15" is.
- **Share broadly.** The blameless write-up goes to all engineers, not just those involved.

## Anti-patterns

- IC also typing commands. Lose the coordinator perspective.
- Silently solving in DMs instead of the channel. Loses the timeline.
- Skipping the status page because "it'll resolve quickly." It won't.
- Skipping the retro because the fix was simple. The simple fix often hid a system issue.

## See also

[[rollback]] · [[deploy-runbook]] · [[oncall-rotation]] · [[../defect-prioritization/severity-classification]]
