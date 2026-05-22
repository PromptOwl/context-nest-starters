---
title: "TypeScript dependency management"
type: document
tags: ["#convention", "#typescript", "#deps"]
status: published
version: 1
---

# TypeScript dependency management

## Tooling

- **pnpm** is the default. Faster install, smaller disk footprint, workspace-native.
- npm and yarn are tolerated for legacy repos. Pick one per repo, never mix lockfiles.
- `engines` field in `package.json` pins the Node version. CI fails if it doesn't match.

## Layers

| Section | Use |
|---|---|
| `dependencies` | Shipped at runtime |
| `devDependencies` | Build tools, linters, tests |
| `peerDependencies` | For libraries — declare what the consumer must provide |
| `optionalDependencies` | Rarely; explain why in a README |

## Pinning policy

- **Lockfile is the truth.** `pnpm-lock.yaml` is committed and reproduces installs deterministically.
- Top-level versions use caret (`^1.2.3`). Patch and minor float; major is a manual bump.
- Exact pinning (`1.2.3`) only when reproducibility specifically requires it (build determinism, security-sensitive packages).
- Upgrades: Renovate or Dependabot, weekly. See [[../../security/dep-vuln-policy]].

## Adding a dependency

1. Search the existing list — `lodash` is the canonical example of "we have this seven times."
2. Check bundle size (`bundlephobia.com` or `pnpm why`). A 200KB transitive for a 5-line helper is a red flag.
3. Check license (MIT / BSD / Apache 2.0 ok; GPL / AGPL needs legal review).
4. `pnpm add <package>` (or `-D` for dev).
5. Verify the lockfile diff is reasonable.
6. Commit `package.json` + `pnpm-lock.yaml` together.

## Vulnerability response

- `pnpm audit` runs in CI. Critical/high CVEs block the PR.
- See [[../../security/dep-vuln-policy]] for SLAs by severity.

## Monorepo notes

- pnpm workspaces (`pnpm-workspace.yaml`) over Lerna or Nx unless tooling demands otherwise.
- Internal packages use `workspace:*` protocol.
- Don't publish internal packages by accident — set `"private": true` everywhere.

## Antipatterns

- Mixing npm + pnpm lockfiles in the same repo.
- `--force` or `--ignore-scripts` on install in CI. Either fix the package or remove it.
- Pinning everything in `package.json` (defeats the lockfile).

## See also

[[style-guide]] · [[../../security/dep-vuln-policy]]
