# Stakeholders

People and teams the GRC platform PM coordinates with. Each role file holds names, contacts, and notes on what they own and how they prefer to be engaged.

## Roles

| Code | Role | File | Owner-side question they answer |
|---|---|---|---|
| BD | Business Development (CPD) | [bd.md](bd.md) | "Who are we selling this to and what did we promise?" |
| CS | Customer Support | [cs.md](cs.md) | "What are users actually hitting in production?" |
| CSC | Customer Success Coach | [csc.md](csc.md) | "Are existing customers getting value? What's at risk?" |

## When to consult whom

- Writing a PRD → BD (commitments), CS (current pain), CSC (existing-customer risk)
- Triaging a bug → CS (frequency, severity)
- Pricing or packaging change → BD primary, CSC secondary
- Onboarding flow change → CSC primary, CS secondary

## Conventions

- Each role file lists contacts as a Markdown table: `| Name | Contact | Notes |`
- Use the role code (BD / CS / CSC) when tagging in PRD or issue text
- Keep contact info minimal — link to the system of record (HRIS, directory) when possible
