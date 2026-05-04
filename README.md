# Annona Docs

Customer-facing documentation hub for Annona.

This repository publishes customer help only.

## What belongs here

- getting started guides for customer tenants
- help pages linked from the customer UI
- customer-safe product explanations for onboarding, agents, datasets, uploads, traces, and account access
- stable help-link targets that product teams can wire into the customer experience

## What does not belong here

- internal runbooks
- platform architecture notes
- operator or admin procedures
- deployment instructions
- private delivery proofs
- DEV-only engineering validation notes

Those internal materials belong in Annona's internal documentation surfaces, especially the Technology Hub in Notion and `goAnnona/annona-architect-flow`.

## Local preview

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```
