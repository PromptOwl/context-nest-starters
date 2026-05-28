---
title: "Service design checklist"
type: document
tags: ["#convention", "#architecture", "#service-design"]
status: published
version: 1
---

# Service design checklist

When you're designing a new service (or a major change to one), walk this list before writing the ADR. The questions you can't answer are the ones the ADR needs to settle.

## Boundaries
- [ ] What does this service own? What does it explicitly NOT own?
- [ ] What's the input boundary (HTTP / gRPC / events / scheduled)?
- [ ] What's the output (responses / events emitted / downstream calls)?
- [ ] What's the trust boundary (who can call it; what auth)?
- [ ] How does it fit the existing service mesh? Pictures help.

## Data
- [ ] What does it persist? Where (which datastore)?
- [ ] What's the consistency model (strong / eventual / monotonic)?
- [ ] Is any of the data PII? See [[../security/pii-handling]] if so.
- [ ] What's the backup story? Restore story?
- [ ] What's the retention policy?

## Failure
- [ ] What happens when downstream is down? (Fallback, queue, fail fast.)
- [ ] What happens under load? (Backpressure, queue limits, rate limits.)
- [ ] Idempotency: retries safe? If not, why not?
- [ ] What's the on-call ergonomics? Are dashboards + alerts ready?
- [ ] How do you roll back if a deploy goes bad? See [[../operations/rollback]].

## Observability
- [ ] Structured logs for every request with the standard fields.
- [ ] Metrics: RED (rate, errors, duration) or USE (utilization, saturation, errors).
- [ ] Traces: spans for every external call.
- [ ] Alerts on the symptoms users care about (success rate, latency), not the causes.

## Security
- [ ] Threat model (see [[../security/threat-modeling]]) for new trust boundaries.
- [ ] Secrets are managed per [[../security/secrets-management]].
- [ ] Authn / authz strategy documented.
- [ ] Input validation at the boundary.
- [ ] Dependency vulnerabilities scanned per [[../security/dep-vuln-policy]].

## Lifecycle
- [ ] Who's the owner (team + on-call)?
- [ ] What's the deprecation policy if this service goes away?
- [ ] What's the test strategy (see [[../qa/test-strategy]])?

## Done when

The team can sign off on every line above. If you can't, those are the ADR's open questions to settle.

## See also

[[adr-template]] · [[api-design]] · [[../operations/deploy-runbook]]
