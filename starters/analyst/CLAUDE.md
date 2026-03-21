# Analyst Research Vault

This is a Context Nest vault for research and analysis — methodologies, source management, and report templates.

## What This Vault Is For

You are working with an analyst who uses this vault to:
- Document and standardize research methodologies
- Catalog data sources with reliability assessments
- Maintain report templates for consistent, professional output
- Accelerate analysis workflows with structured context

## Vault Structure

- `nodes/methodologies/` — Research frameworks, analysis procedures, quality criteria, workflow guides
- `nodes/sources/` — Source catalogs, data provider evaluations, access procedures, reliability ratings
- `nodes/reporting/` — Report templates, formatting standards, review checklists, presentation guides

## How to Use Context Nest Commands

- `ctx search <query>` — Find specific content across the vault
- `ctx resolve <selector>` — Query by tags, type, or status (supports AND/OR/NOT)
- `ctx add <path>` — Create a new document
- `ctx update <path>` — Modify an existing document
- `ctx inject <selector>` — Inject matching context into AI conversations
- `ctx verify` — Validate integrity of all documents

## First 5 Things To Do

1. **Read the existing documents** in `nodes/` to understand the templates
2. **Ask the user about their analysis domain** — what do they research? What are their deliverables?
3. **Help them customize the methodology template** for their specific workflow
4. **Build a source catalog** documenting the data sources they actually use
5. **Create a report template** matching their organization's requirements

## Working Style

- Accuracy matters more than speed — always flag uncertainty
- Cite sources and document reasoning chains
- When drafting reports, maintain professional tone and structure
- Use the YAML frontmatter format (title, type, tags, priority, status)
- Distinguish between findings (factual) and analysis (interpretation)

---
Vault powered by [Context Nest](https://github.com/PromptOwl/ContextNest) by PromptOwl — promptowl.ai
