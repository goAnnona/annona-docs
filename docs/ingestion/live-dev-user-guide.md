# Live DEV ingestion user guide

This guide shows the current route-based Annona onboarding and ingestion flow:

1. open `/ingestion`
2. choose the target customer plane
3. upload or stage one accepted bundle/source file
4. validate the destination preview and expected dataset naming
5. submit through the guided workflow or the expert fast path
6. inspect durable dataset and customer detail on `/datasets` and `/customers`
7. verify downstream grounded answers and traces with the correct DEV disclosure

## Scope and guardrails

- This is a **DEV-only proof flow**.
- `/ingestion` is the canonical onboarding and upload route; older proof-only query-flag onboarding references are transitional and should not be used in user instructions.
- `/datasets` and `/customers` now own durable dataset and customer detail instead of duplicated proof panels inside onboarding.
- Live lake access is exposed as a **DEV Annona lake preview built from customer-uploaded data**. It is **not** direct ERP or warehouse access.
- If the data-lake bridge URLs are not configured, the UI falls back to deterministic DEV snapshots and disables live submission except in Playwright test mode.

## Validated route structure

| Route | What to look for |
| --- | --- |
| `/ingestion` | Customer ingestion workspace, current stage, next action, latest outcome, guided steps, upload CTA, validation preview, expert fast path, selected dataset readiness, and links to compare/datasets/runs. |
| `/datasets` | Dataset discovery workspace with version list, schema preview, row preview, lineage, freshness, and related activity. |
| `/customers` | Customer context workspace with onboarding stage, connector inventory, latest intake health, linked datasets, and linked runs. |
| `/runs` | Run inspection workspace for traces, tool calls, evidence refs, timings, and failures. |
| `/` or `/compare` | Presentation-ready side-by-side comparison without embedded upload forms. |

## 1) Authenticate into the DEV app

Open the DEV app and authenticate with the current demo password. The comparison route stays clean for the product story; operator onboarding now lives on `/ingestion`.

## 2) Open the guided onboarding workflow

Navigate to `/ingestion`. The page should show **Customer ingestion workspace**, a current-stage card, a next-action card, a latest-outcome card, and the ordered ingestion steps.

![Guided ingestion wizard on /ingestion](../assets/ingestion/current-ux/01-ingestion-wizard.png)

The ordered workflow is:

1. Customer selection
2. File selection
3. Validation and preview
4. Confirmation
5. Submission
6. Completion

## 3) Select a customer and file

Choose the target customer plane, then select one accepted archive or source file. The UI previews the target plane, expected dataset naming, raw intake root, validation state, and submission mode.

When safe defaults are satisfied, the workflow shows that the **Expert fast path** is available. This path is intended for repeat operators who do not need the extra confirmation click.

![Expert fast path available after safe file selection](../assets/ingestion/current-ux/02-expert-fast-path-ready.png)

## 4) Submit and watch completion

Use either **Confirm and submit** or **Expert fast path**. The page should move to **Completion** and tell the operator to refresh status until landing, parsing, and discovery finish.

![Fast path submitted in Playwright test mode](../assets/ingestion/current-ux/03-fast-path-submitted.png)

In a live DEV deployment with the data-lake bridge configured, the submit path returns an intake ID and status refreshes show the bridge-backed ingestion outcome. In local/test-mode screenshots, the visible `Playwright submit mock` banner is expected and does not claim live bridge persistence.

## 5) Inspect durable dataset detail

Open `/datasets` from the shell or an ingestion deep link when you need dataset-version ownership details. This route owns version lists, schema preview, row preview, lineage, freshness, and related activity.

![Dedicated datasets workspace on /datasets](../assets/ingestion/current-ux/04-datasets-workspace.png)

## 6) Inspect customer context

Open `/customers` when you need customer-plane detail. This route owns onboarding stage, connector inventory, latest intake health, linked datasets, and linked runs.

![Dedicated customers workspace on /customers](../assets/ingestion/current-ux/05-customers-workspace.png)

## 7) Verify grounded use of uploaded data

After a live DEV upload has produced a discoverable dataset version, use `/compare` and `/runs` to verify the grounded path. The expected proof signals are:

- the latest intake reaches a successful or explicitly reviewable terminal state
- the selected dataset shows the expected `dataset_version_id`
- dataset record counts and previews match the uploaded file or bundle
- grounded answers disclose that the source is a live DEV Annona lake preview built from customer-uploaded data
- traces show the run/session, dataset, timing, tool, evidence, and request context needed for operator review

## What this proves today

The current DEV demo supports the user-visible proof chain:

- route-based onboarding on `/ingestion`
- guided upload and validation with an expert fast path
- live handoff into the Annona data-lake bridge when configured
- processing into discoverable dataset versions
- durable dataset and customer pages on `/datasets` and `/customers`
- grounded-agent access to uploaded data with explicit DEV-preview disclosure

## What this does **not** claim yet

- direct production ERP connectivity
- direct warehouse federation
- production support or SLAs
- operator-free autonomous ingestion repair
- live upload submission when the data-lake bridge URLs are not configured
