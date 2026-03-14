# Team Knowledge Vault

This is a Context Nest vault for team knowledge — shared processes, handbook, and onboarding.

## What This Vault Is For

You are working with a team that uses this vault to:
- Document how the team works — processes, rituals, norms
- Maintain a living handbook that new hires actually read (because AI surfaces it)
- Keep onboarding materials current and accessible
- Preserve institutional knowledge so it doesn't leave when people do

## Vault Structure

- `nodes/processes/` — How we work: meeting cadences, decision-making, escalation, communication norms
- `nodes/knowledge/` — Team handbook: values, tools, glossary, FAQ, tribal knowledge
- `nodes/onboarding/` — New hire guides: first day, first week, first month checklists and orientation

## How to Use Context Nest Commands

- `ctx search <query>` — Find specific content across the vault
- `ctx resolve <selector>` — Query by tags, type, or status
- `ctx add <path>` — Create a new document
- `ctx update <path>` — Modify an existing document
- `ctx inject <selector>` — Inject team knowledge into AI conversations
- `ctx verify` — Validate integrity of all documents

## First 5 Things To Do

1. **Read the existing documents** to understand the template structure
2. **Ask the user about their team** — how many people? What do they do? What tools do they use?
3. **Help them document their core processes** — meeting structure, decision-making, communication channels
4. **Build a team FAQ** capturing the questions new hires always ask
5. **Create an onboarding checklist** for the first day, first week, and first month

## Working Style

- Be clear and welcoming — this content is often the first thing a new hire reads
- Avoid jargon unless you define it in a glossary
- Keep documents short and scannable — bullet points over paragraphs
- Use the YAML frontmatter format (title, type, tags, priority, status)
- Suggest updates when you notice content might be outdated

---
Vault powered by [Context Nest](https://github.com/PromptOwl/context-nest) by PromptOwl — promptowl.ai
