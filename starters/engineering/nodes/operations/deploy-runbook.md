---
title: "Deploy runbook"
type: document
tags: ["#runbook", "#operations", "#deploy"]
status: published
version: 1
---

# Deploy runbook

## Pre-deploy checklist

- [ ] PR has been reviewed and approved per [[../process/code-review-checklist]].
- [ ] CI is green on `main`. Don't deploy from a red CI; fix it first.
- [ ] Migrations (if any) have been reviewed for safety: no `ALTER TABLE` on hot tables without `CONCURRENTLY`, no destructive operations without a rehearsed rollback.
- [ ] Feature flags: anything risky is gated behind a flag, default off, with a kill switch documented.
- [ ] On-call is aware. Quick Slack heads-up to `#deploys` or equivalent.
- [ ] No competing deploy in flight (one at a time per service).

## Deploy

Substitute the team's actual tool — this section gets edited per repo.

1. Tag the release: `git tag v<semver> && git push --tags`.
2. CI pipeline runs: build artifact → run smoke tests → push to registry.
3. Deploy to **canary** (1–5% of traffic). Wait 15 minutes.
4. Watch the dashboards: error rate, latency P99, saturation. Compare to baseline.
5. Promote to **full rollout** if signals are clean.
6. Update the deploy log (or whatever tracks "what's in prod right now").

## During the deploy

Engineer pushing the button stays at the keyboard until the rollout is complete. Don't start a deploy and walk away.

## Post-deploy

- [ ] Confirm the version reported by `/health` (or equivalent) matches.
- [ ] Spot-check user-visible behavior: log into the app, perform the critical action.
- [ ] Update the changelog or release notes if user-facing.
- [ ] Close the deploy thread / Slack post.

## If something goes wrong

Don't roll forward to fix. **Roll back first**, then fix in a follow-up branch. See [[rollback]].

The asymmetry: rolling back returns the system to a known state. Rolling forward stacks unknowns on top of an already-broken state.

## Deploy windows

- Default: any business hour, Monday–Thursday.
- Avoid: Friday afternoons (no one to fix it before the weekend), holidays, before major events.
- Hotfix exception: any time, with explicit "I am the on-call for this push" sign-off.

## Anti-patterns

- Deploying changes you didn't write or review.
- Skipping canary for "just a quick fix."
- Deploying with known-flaky tests disabled to get CI green.
- Coordinating deploys via DM. Use a public channel.

## See also

[[rollback]] · [[ir-template]] · [[oncall-rotation]]
