---
title: "Rollback procedure"
type: document
tags: ["#runbook", "#operations", "#rollback"]
status: published
version: 1
---

# Rollback procedure

## When to roll back (not when to debug)

**Roll back first if any of these are true:**

- Error rate above baseline by > 2x.
- P99 latency above baseline by > 50%.
- A user-visible breakage reported by more than one source.
- Data corruption observed or strongly suspected.
- The deploy introduced a security regression.

**Don't roll back if:**

- Rolling back would lose data (a migration committed) — see "Forward-fix" below.
- The issue predates the deploy (verify by checking the timeline).
- A feature flag toggle fixes it. Toggle the flag instead.

## The rollback decision is a 60-second decision

Don't deliberate. The product invariant: a known-bad version is always worse than a known-good previous version. Roll back; investigate after.

## The procedure

Substitute the team's actual tool. The shape is the same:

1. **Announce.** Post in the deploy channel: "Rolling back v1.2.3, observed X."
2. **Execute the rollback command.** Should be one command — `kubectl rollout undo`, `aws deploy stop-deployment`, `terraform apply` of the prior tag, whatever the platform provides.
3. **Verify.** Check `/health` reports the previous version. Check dashboards return to baseline.
4. **Communicate.** Update the channel: "Rolled back to v1.2.2. Investigating."

Total time, from observed regression to verified rollback: target < 5 minutes.

## Forward-fix path (when rollback isn't safe)

If a migration ran and rolling back would lose data:

1. Determine what's broken.
2. Write the smallest possible patch.
3. Deploy through the same channel (canary → full) with on-call standby.
4. Document why rollback wasn't an option in the post-incident report.

This is rare and risky. Most "we can't roll back" turns out to be "we don't want to write the migration's reverse" — which means the reverse should be written and rehearsed *before* the next risky deploy.

## Decision tree

```
Regression observed
       │
       ▼
Was the last deploy < 30 min ago AND no destructive migration ran?
       │
   yes ├──── ROLL BACK NOW
       │
   no  ▼
Is there a feature flag we can toggle?
       │
   yes ├──── TOGGLE; continue investigation
       │
   no  ▼
FORWARD-FIX with on-call standby + post-incident review.
```

## Post-rollback

- File an incident retrospective. Use [[ir-template]].
- Add a regression test for the bug the rollback dodged.
- Don't reland the bad change until the test exists AND someone reviewed it specifically.

## Anti-patterns

- "Let me try one more thing" instead of rolling back. 5 minutes turns into an hour.
- Rolling back without telling anyone. The next engineer to deploy steps on it.
- Skipping the retrospective because "we rolled back, no harm done." Harm: the team learned nothing.

## See also

[[deploy-runbook]] · [[ir-template]] · [[oncall-rotation]]
