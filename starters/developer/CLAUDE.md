# Developer Project Vault

This is a Context Nest vault for engineering — architecture decisions, coding standards, and project knowledge.

## What This Vault Is For

You are working with a developer or engineering team that uses this vault to:
- Document architecture decisions and their rationale
- Maintain coding standards and conventions
- Accelerate onboarding for new team members
- Keep project knowledge structured and injectable into AI conversations

## Vault Structure

- `nodes/architecture/` — Architecture Decision Records (ADRs), system design docs, tech stack rationale
- `nodes/standards/` — Coding conventions, review guidelines, testing practices, naming patterns
- `nodes/onboarding/` — Setup guides, project overview, key contacts, "how things work" docs

## How to Use Context Nest Commands

- `ctx search <query>` — Find specific content across the vault
- `ctx resolve <selector>` — Query by tags, type, or status (supports AND/OR/NOT)
- `ctx add <path>` — Create a new document
- `ctx update <path>` — Modify an existing document
- `ctx inject <selector>` — Inject matching context into AI conversations
- `ctx verify` — Validate integrity of all documents
- `ctx history <path>` — View version timeline of a document

## First 5 Things To Do

1. **Read the existing documents** in `nodes/` to see the templates and examples
2. **Ask the user about their project** — what's the stack, what are they building?
3. **Help them fill in the architecture overview** with their actual tech stack and decisions
4. **Create a coding standards doc** based on their preferences and existing codebase patterns
5. **Set up an onboarding doc** that would help a new developer get productive in this project

## Working Style

- Be technical and precise — developers want specifics, not hand-waving
- When creating ADRs, include: context, decision, consequences, alternatives considered
- Use code examples where relevant
- Always use the YAML frontmatter format (title, type, tags, priority, status)
- Suggest vault improvements when you notice knowledge gaps

---
Vault powered by [Context Nest](https://github.com/PromptOwl/ContextNest) by PromptOwl — promptowl.ai
