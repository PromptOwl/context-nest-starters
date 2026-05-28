---
title: "Git flow — trunk-based"
type: document
tags: ["#convention", "#process", "#git"]
status: published
version: 1
---

# Git flow — trunk-based

This starter ships with **trunk-based development** as the default. Edit this node if the team uses GitHub flow, git-flow, or something else — the doc is the team's truth.

## The model

- One long-lived branch: `main`.
- Every change ships through a short-lived feature branch (hours to a few days, never weeks).
- Releases happen from `main`. Tags are immutable.
- Hotfixes are also feature branches; not a separate hierarchy.

## Branch naming

`<author>/<ticket-id>/<short-description>`

- `<author>` — usually a username or initials. Helps the team know who owns a branch at a glance.
- `<ticket-id>` — link to the work tracker (Linear, ClickUp, GitHub issue). Embedded so the tooling can auto-link.
- `<short-description>` — kebab-case, ≤4 words.

Example: `alice/CU-86abc/fix-checkpoint-dedup`

## Lifecycle

1. Branch from current `main` (`git pull --ff-only main && git checkout -b ...`).
2. Commit early, commit often on the feature branch.
3. Push and open a PR as soon as there's something to discuss — drafts are fine.
4. Squash-merge to `main` when approved. The squash message is the PR title + body.
5. Delete the branch after merge.

## Anti-patterns

- Long-lived feature branches. Anything > 5 days needs a daily-rebase ritual or it'll conflict-storm at merge.
- Force-pushing `main`. Forbidden. Protected by branch rules.
- Merging without review. Forbidden. The exception is solo-developer repos with documented self-review discipline.
- Commit messages like "fix" or "wip" surviving into `main`. The squash message is the only one that matters; make it good.

## See also

[[pr-template]] · [[commit-conventions]] · [[code-review-checklist]]
