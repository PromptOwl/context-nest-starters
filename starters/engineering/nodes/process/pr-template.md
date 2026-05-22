---
title: "Pull request template"
type: document
tags: ["#convention", "#process", "#pr"]
status: published
version: 1
---

# Pull request template

Copy this template to `.github/pull_request_template.md` (GitHub), `merge_request_templates/` (GitLab), or wherever the tool expects it. Customize sections per repo.

```markdown
## Summary
- One-line description of what changed and why.
- Bullet 2: any non-obvious decisions.
- Bullet 3: links to ticket / ADR / preceding PR.

## What changed
- File / module touchpoints.
- New dependencies, if any.
- Migrations, if any.

## Why this approach
Brief — only if there's a non-obvious trade-off. Otherwise omit.

## Test plan
- [ ] Unit tests pass locally.
- [ ] Integration / E2E tests run (if applicable).
- [ ] Manual verification: <steps>
- [ ] Documentation updated: <where>

## Risks
- What could go wrong?
- What's the rollback story?

## Out of scope
What this PR intentionally does NOT do. Prevents review scope creep.
```

## Discipline

- **One PR, one concern.** Don't bundle unrelated changes. Reviewers can't sign off on what they can't focus on.
- **Description matches the diff.** The PR body should let a reviewer understand the change without reading every file.
- **Test plan is checked, not aspirational.** Don't tick a box you haven't actually run.
- **Out-of-scope section sets the contract.** Anything in the diff that's NOT in the description is a surprise — surprises slow review.

## See also

[[code-review-checklist]] · [[commit-conventions]] · [[git-flow]]
