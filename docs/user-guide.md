# Annona user guide

This guide describes the current DEV web UI route structure and the primary jobs each surface owns.

## Route map

| Route | Purpose | Use it for |
| --- | --- | --- |
| `/` or `/compare` | Clean side-by-side comparison surface | Run prompts and compare raw versus Annona-grounded answers. |
| `/ingestion` | Guided customer onboarding and bundle ingestion | Select a customer, stage a file, validate the destination, submit ingestion, and track completion. |
| `/datasets` | Durable dataset discovery workspace | Inspect dataset versions, schema preview, row preview, lineage, freshness, and related activity. |
| `/customers` | Durable customer context workspace | Review onboarding stage, connector inventory, latest intake health, linked datasets, and linked runs. |
| `/runs` | Trace and run inspection workspace | Inspect run/session context, tool calls, evidence refs, timings, and failures. |

The old proof-only onboarding workspace is no longer the canonical user path. Use `/ingestion` for onboarding and upload work, then deep-link into `/datasets`, `/customers`, `/runs`, or `/compare` as needed. For the proven side-by-side demo behavior, see [DEV comparison demo and trace surfaces](proofs/dev-comparison-demo.md).

## Standard workflow

1. Authenticate into the DEV app with the current demo password.
2. Open `/ingestion` and choose the target customer plane.
3. Select one accepted archive or source file.
4. Review validation, destination preview, and expected dataset naming.
5. Submit through the guided path, or use the expert fast path when safe defaults are already satisfied.
6. Refresh status until landing, parsing, discovery, and readiness are reflected in the latest outcome.
7. Open `/datasets` for durable dataset detail, `/customers` for customer plane detail, `/runs` for trace review, or `/compare` to use the selected context in the comparison story.

## DEV scope and disclosure

- Annona currently presents this as a DEV proof workflow, not a production service with SLAs.
- Live uploaded-data access is disclosed as a **live DEV Annona lake preview built from customer-uploaded data**.
- It is **not** direct ERP access, direct warehouse federation, or a claim that Annona can read arbitrary customer systems in production.
- Live submit requires `ANNONA_DATA_LAKE_SUBMIT_URL` and `ANNONA_DATA_LAKE_STATUS_URL` in the deployed web runtime.
- When the web UI is running without the data-lake bridge, the onboarding surfaces fall back to deterministic DEV snapshots and live submission is disabled except in Playwright test mode.

## Validated artifacts

The available live successful-submit screenshots were captured from the earlier `/?proof=1` proof workspace before the current route split. They are retained as live proof artifacts because they show a real accepted intake, resulting dataset version, and grounded use of uploaded data.

![Upload accepted in the live DEV proof workspace](assets/ingestion/live-e2e/03-upload-accepted.png)

![Lake-backed dataset ready for analysis in the live DEV proof workspace](assets/ingestion/live-e2e/04-lake-dataset-ready.png)

The current route screenshots below were captured from the shipped web UI route contract in Playwright test mode. The visible banners intentionally say `Mocked DEV snapshot` and `Playwright submit mock`; they validate the current UX structure and copy, not a fresh live bridge upload.

![Guided ingestion wizard on /ingestion](assets/ingestion/current-ux/01-ingestion-wizard.png)

![Expert fast path available after safe file selection](assets/ingestion/current-ux/02-expert-fast-path-ready.png)

![Dedicated datasets workspace on /datasets](assets/ingestion/current-ux/04-datasets-workspace.png)

![Dedicated customers workspace on /customers](assets/ingestion/current-ux/05-customers-workspace.png)
