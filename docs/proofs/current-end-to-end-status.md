# Current end-to-end status

This page is the landing page for Annona proof documentation.

## Proven today

- DEV web UI delivery path from PR merge to deploy to smoke to proof
- Live side-by-side comparison demo in DEV on `/` and `/compare`
- Route-based onboarding and upload workflow on `/ingestion`
- Guided ingestion stepper with customer selection, file selection, validation and preview, confirmation, submission, and completion
- Expert fast path for repeat operators when safe defaults are satisfied
- Live browser upload into the DEV data-lake bridge when bridge URLs are configured
- Bridge-backed job polling through customer status with `submitted`, `running`, `completed`, `completed_with_errors`, and `failed` states
- RAW, BRONZE, manifest, and DuckDB status/discovery outputs for accepted DEV customer bundles
- Dedicated `/datasets` workspace for dataset versions, schema preview, row preview, lineage, freshness, and related activity
- Dedicated `/customers` workspace for customer onboarding status, connector inventory, latest intake health, linked datasets, and linked runs
- Grounded answer rendering with visible evidence and trace surfaces
- Discoverable lake-backed dataset versions for uploaded customer data
- Grounded-agent access to uploaded datasets through the live DEV preview path with explicit disclosure

## Primary proof guides

- [Annona user guide](../user-guide.md)
- [DEV comparison demo and trace surfaces](dev-comparison-demo.md)
- [DEV delivery loop runbook](../delivery/dev-delivery-loop.md)
- [Current live ingestion status](../ingestion/current-live-status.md)
- [Live DEV ingestion user guide](../ingestion/live-dev-user-guide.md)

## Screenshot provenance

The current docs intentionally carry two classes of ingestion screenshots:

- live proof artifacts from the earlier `/?proof=1` workspace, showing a real accepted submit, resulting dataset version, and grounded use of uploaded data
- Playwright/mock UX captures for the current `/ingestion`, `/datasets`, and `/customers` route structure after the route split

Use the [Live DEV ingestion user guide](../ingestion/live-dev-user-guide.md) for the full distinction and current operator steps.
