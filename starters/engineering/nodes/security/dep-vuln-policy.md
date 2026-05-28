---
title: "Dependency vulnerability policy"
type: document
tags: ["#convention", "#security", "#deps"]
status: published
version: 1
---

# Dependency vulnerability policy

## SLAs by severity

| Severity (CVSS) | Patch SLA | Workflow |
|---|---|---|
| **Critical (≥ 9.0)** | 24 hours | Drop everything; hotfix + immediate deploy |
| **High (7.0–8.9)** | 7 days | Open PR within 48h; ship within the week |
| **Medium (4.0–6.9)** | 30 days | Roll into the next regular upgrade cycle |
| **Low (< 4.0)** | 90 days | Roll into quarterly maintenance |

The clock starts when the advisory is published, not when the team becomes aware.

## Scanning

- **CI scan on every PR.** Critical/high CVEs in *touched* dependencies block the PR.
- **Daily scan on `main`.** New advisories against existing deps file a ticket automatically.
- Per language: `pip-audit` / `npm audit` / `pnpm audit` / `cargo audit` / `govulncheck` / `mvn dependency-check`.

## Reachability matters

A vulnerability in a transitive dependency that your code doesn't actually exercise is *lower* risk than one in a direct path. Tools that surface reachability (Snyk's "Snyk Code", `govulncheck`'s analysis) get used to prioritize.

- **Reachable** + critical: hotfix SLA.
- **Not reachable** + critical: regular upgrade cadence with a documented suppression.

Document the reachability analysis in the suppression entry so the next reviewer sees the reasoning.

## Suppression

- Suppressions are temporary. Maximum 6 months.
- Every suppression has a comment explaining why and a ticket linking to the long-term fix (replace dependency, upgrade major version, etc.).
- Quarterly review: walk every suppression, kill the ones that can't be justified.

## Upgrade cadence (no-CVE baseline)

Independent of CVEs, dependencies drift. Weekly automated PR run (Dependabot / Renovate) ensures the lockfile doesn't ossify.

- Patch upgrades: auto-merge after CI passes.
- Minor upgrades: human review before merge.
- Major upgrades: tracked as work items, not auto-PRs.

## License compliance

License and security are scanned together. Allowed: MIT, BSD-2/3, Apache-2.0, MPL-2.0, ISC. Flagged for legal review: GPL, AGPL, SSPL, proprietary. Maintain `deny.toml` (Rust), `license-checker` config (JS), or equivalent.

## See also

[[threat-modeling]] · [[secrets-management]] · [[../languages/python/deps]] · [[../languages/typescript/deps]]
