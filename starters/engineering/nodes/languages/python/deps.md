---
title: "Python dependency management"
type: document
tags: ["#convention", "#python", "#deps"]
status: published
version: 1
---

# Python dependency management

## Tooling

- **uv** for managing project dependencies. Fast, lockfile-first, replaces pip + pip-tools + virtualenv.
- `pyproject.toml` declares dependencies; `uv.lock` pins them. Both committed.
- No `requirements.txt` in new projects. Legacy repos can keep one but treat `pyproject.toml` as the source.

## Layers

| Layer | Where it lives | Examples |
|---|---|---|
| Runtime | `[project] dependencies` | fastapi, sqlalchemy, pydantic |
| Dev | `[dependency-groups] dev` | pytest, ruff, mypy |
| Optional | `[project.optional-dependencies]` | extras users opt into |

## Pinning policy

- **Lockfile is the truth.** `uv sync` reproduces the exact environment across machines and CI.
- Top-level dependencies use minimum-version constraints (`>=1.2,<2`). Patch versions float.
- Transitive dependencies are pinned exactly in the lockfile but not in `pyproject.toml`.
- Upgrade cadence: weekly automated PR via Dependabot or Renovate. See [[../../security/dep-vuln-policy]].

## Adding a dependency

1. Check the existing list — duplicates and rough equivalents (httpx vs requests) are common.
2. Verify license (MIT / BSD / Apache 2.0 OK; GPL / AGPL flagged for legal review).
3. Run `uv add <package>` (or `uv add --dev <package>` for dev-only).
4. Confirm lockfile diff is reasonable (one new entry, plus transitives).
5. Commit `pyproject.toml` + `uv.lock` together.

## Vulnerability response

- `uv audit` (or `pip-audit`) runs in CI. Any CVE blocks the PR.
- High/critical CVEs: patch within the SLA defined in [[../../security/dep-vuln-policy]].
- Medium/low: roll into the next scheduled upgrade.

## Antipatterns

- `pip install` directly into a shared environment.
- Pinning everything in `pyproject.toml` (defeats the lockfile).
- Adding a dependency for a 20-line helper. Write the helper.

## See also

[[style-guide]] · [[../../security/dep-vuln-policy]]
