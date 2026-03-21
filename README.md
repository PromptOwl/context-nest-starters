# Context Nest Starters

> Free starter vaults for [Context Nest](https://github.com/PromptOwl/ContextNest) — role-based templates that get you and your AI productive in 30 seconds.

## What Are Starters?

A starter is a pre-built vault template for a specific role. When you run `ctx init --starter <name>`, you get:

- A **folder structure** tailored to your work
- **Example documents** with real frontmatter you can customize
- A **CLAUDE.md** that teaches AI agents how to use your vault immediately
- A **"first 5 things to do"** checklist your AI walks through with you

No setup. No configuration. Your AI just *knows* your vault from the first conversation.

## Available Starters

| Starter | Who It's For | What You Get |
|---------|-------------|-------------|
| [`executive`](starters/executive/) | C-suite, VPs, senior leaders | Strategy, operations, leadership playbooks |
| [`developer`](starters/developer/) | Engineers, tech leads | Architecture decisions, coding standards, onboarding |
| [`analyst`](starters/analyst/) | Research, security, compliance analysts | Methodologies, source catalogs, report templates |
| [`sales`](starters/sales/) | Sales reps, AEs, sales leaders | Objection handling, battlecards, enablement |
| [`team`](starters/team/) | Any team that shares knowledge | Processes, handbooks, onboarding guides |

## Quick Start

```bash
# Install Context Nest CLI
npm install -g contextnest-cli

# Initialize a vault with a starter
ctx init --starter developer

# Start talking to your AI — it already knows your vault
```

## Starter Structure

Each starter contains:

```
starters/<name>/
├── manifest.yaml      # Recipe metadata (name, description, tags)
├── CLAUDE.md           # AI instructions — the bridge between vault and AI
└── nodes/              # Pre-built knowledge documents
    ├── <category>/
    │   └── <document>.md
    └── ...
```

## Using Without the CLI

You can also use starters manually:

1. Copy the starter folder contents into your project
2. The `CLAUDE.md` file will be picked up by Claude Code, Claude Desktop, or any MCP-compatible AI
3. Start adding your own documents to `nodes/`

## Creating Your Own Starter

Want to contribute a starter? See [CONTRIBUTING.md](CONTRIBUTING.md).

A starter needs:
- `manifest.yaml` with name, description, version, author, and tags
- `CLAUDE.md` with role-specific AI instructions
- At least 2-3 example documents in `nodes/` with proper frontmatter

## Cloud Packs

Looking for more than free starters? [PromptOwl Cloud](https://promptowl.ai) offers curated context packs with deeper content, maintained by domain experts. Free tier includes 50 injections/month.

```bash
# Inject a cloud pack (requires PromptOwl account)
ctx inject @promptowl/enterprise-sales-playbook
```

## Links

- [Context Nest CLI](https://github.com/PromptOwl/ContextNest) — the open-source knowledge management system
- [PromptOwl](https://promptowl.ai) — the AI operating system for knowledge
- [Documentation](https://docs.promptowl.ai) — full docs (coming soon)

---

Built by [PromptOwl](https://promptowl.ai) — Turn knowledge into revenue.
