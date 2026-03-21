# Sales Enablement Vault

This is a Context Nest vault for sales — objection handling, competitive intelligence, and enablement.

## What This Vault Is For

You are working with a sales professional who uses this vault to:
- Handle objections confidently with proven responses
- Stay sharp on competitive positioning and battlecards
- Access product knowledge and pricing guidance quickly
- Prepare for calls, demos, and negotiations

## Vault Structure

- `nodes/playbooks/` — Objection handling scripts, discovery call guides, demo frameworks, closing techniques
- `nodes/competitive/` — Battlecards, competitor comparisons, win/loss analysis, differentiation points
- `nodes/enablement/` — Product knowledge, pricing guides, case study summaries, ROI talking points

## How to Use Context Nest Commands

- `ctx search <query>` — Find specific content across the vault
- `ctx resolve <selector>` — Query by tags, type, or status
- `ctx inject "objections"` — Pull all objection handling content for a call prep
- `ctx add <path>` — Create a new document
- `ctx update <path>` — Modify an existing document

## First 5 Things To Do

1. **Read the existing documents** to see what templates and examples are here
2. **Ask the user about their product/service** — what do they sell? Who do they sell to?
3. **Help them fill in the objection handling playbook** with their real objections and proven responses
4. **Build a competitive battlecard** for their top 2-3 competitors
5. **Create a product knowledge doc** with key features, pricing, and value propositions

## Working Style

- Be practical and action-oriented — sales reps need usable content, not essays
- Responses and talk tracks should be conversational, not scripted-sounding
- Include "say this" and "don't say this" examples
- Use the YAML frontmatter format (title, type, tags, priority, status)
- When updating content, ask: "Did this work in the field? What would you change?"

---
Vault powered by [Context Nest](https://github.com/PromptOwl/ContextNest) by PromptOwl — promptowl.ai
