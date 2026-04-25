# Annona Docs

This site is the documentation home for Annona.

## What belongs here

- what works end to end in DEV today
- screenshots and proof guides
- ingestion, tracing, and delivery architecture
- operator runbooks
- environment and deployment notes

## Current status

The Annona comparison demo is live in DEV.

The ingestion path is now route-based: `/ingestion` owns guided onboarding and upload, `/datasets` owns durable dataset detail, `/customers` owns customer context, `/runs` owns trace inspection, and `/` or `/compare` stays focused on the side-by-side comparison story.

Live DEV deployments with the data-lake bridge configured support browser upload, bridge processing into the DEV lake, dataset discovery, and grounded-agent access to uploaded data. Local or unbridged environments show deterministic DEV snapshots and do not claim live persistence.

See:

- [Annona user guide](user-guide.md)
- [Current end-to-end status](proofs/current-end-to-end-status.md)
- [DEV delivery loop runbook](delivery/dev-delivery-loop.md)
- [Current live ingestion status](ingestion/current-live-status.md)
- [Live DEV ingestion user guide](ingestion/live-dev-user-guide.md)
