# Current live ingestion status

## Status

**Live end to end in DEV, with route-based onboarding now canonical.**

The Annona DEV demo supports browser upload, bridge processing into the DEV lake, dataset discovery, and grounded-agent access to uploaded data when the deployed environment is wired to the data-lake bridge.

## Proven today

- onboarding and bundle upload are owned by the dedicated `/ingestion` route
- `/ingestion` exposes a six-step guided workflow: customer selection, file selection, validation and preview, confirmation, submission, and completion
- expert users can use the fast path when the selected customer, file, naming, and destination defaults are already safe
- `/datasets` owns durable dataset versions, schema preview, row preview, lineage, freshness, and related activity
- `/customers` owns customer onboarding stage, connector inventory, latest intake health, linked datasets, and linked runs
- grounded-agent traces can prove lake-backed preview access to uploaded customer data with explicit DEV disclosure

## Current operator guide

See the full route-based guide here:

- [Annona user guide](../user-guide.md)
- [Live DEV ingestion user guide](live-dev-user-guide.md)

## Important disclosure

Live dataset access in this demo is described as a **live DEV Annona lake preview built from customer-uploaded data**. It is **not** direct ERP or warehouse access.

If the web UI is running without `ANNONA_DATA_LAKE_STATUS_URL` and `ANNONA_DATA_LAKE_SUBMIT_URL`, the onboarding pages show deterministic DEV snapshot data and live upload submission is disabled outside Playwright test mode. Screenshots that display `Mocked DEV snapshot` or `Playwright submit mock` validate the UX structure, not live bridge persistence.
