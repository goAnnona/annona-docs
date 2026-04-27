# Annona Docs

Customer-facing documentation hub for Annona.

This repo should publish customer help only.

## What belongs here

- getting started guides for customer tenants
- help pages linked from the customer UI
- product explanations for onboarding, agents, datasets, traces, and account access
- customer-safe troubleshooting and FAQ content

## What does not belong here

- internal runbooks
- platform architecture notes
- operator or admin procedures
- deployment instructions
- private delivery proofs

Those internal materials belong in `goAnnona/annona-architect-flow` or other private/internal repos.

## Local preview

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```
