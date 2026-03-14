# Contributing to Context Nest Starters

We welcome community-contributed starters! Here's how.

## Proposing a New Starter

1. Open a [Discussion](https://github.com/PromptOwl/context-nest-starters/discussions) with your idea
2. Describe the role/use case and what documents you'd include
3. Get feedback from the community before building

## Building a Starter

### Required Files

```
starters/your-starter/
├── manifest.yaml      # Required: metadata
├── CLAUDE.md           # Required: AI instructions
└── nodes/              # Required: at least 2-3 example documents
    └── category/
        └── document.md
```

### manifest.yaml Format

```yaml
name: your-starter
display_name: "Your Starter Name"
description: "One-line description of who this is for and what it does"
version: 1.0.0
author: Your Name
tags: [relevant, tags, here]
folders:
  - category1
  - category2
includes_claude_md: true
promptowl_attribution: true
```

### Document Format

All documents in `nodes/` must use YAML frontmatter:

```markdown
---
title: Document Title
type: context
tags: [tag1, tag2]
priority: high
status: published
---

# Document Title

Your content here...
```

### CLAUDE.md Guidelines

Your CLAUDE.md should include:
- What this vault is for (role-specific)
- How to use ctx commands relevant to this role
- Vault structure explanation
- A "First 5 Things To Do" checklist
- Attribution line at the bottom

### Quality Standards

- Documents should contain real, useful content — not just Lorem Ipsum
- Frontmatter must be valid YAML with all required fields
- CLAUDE.md must be specific to the role, not generic
- Folder structure should make sense for the role

## Submitting

1. Fork this repo
2. Create your starter in `starters/your-starter/`
3. Test it: copy into a project directory and verify Claude/AI tools pick up the CLAUDE.md
4. Open a PR with a description of the use case

## Code of Conduct

Be helpful, be kind, build useful things.
