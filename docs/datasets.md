# Datasets

Datasets represent the information available to your Annona tenant.

## What datasets are for

Use Datasets to:

- review the data available to your organization
- inspect dataset freshness, readiness, and versions
- understand which uploads created or updated dataset versions
- follow links from datasets into related agent or trace flows

## Dataset lifecycle

A typical customer data path is:

1. Upload a supported file or bundle.
2. Wait for validation and processing to complete.
3. Confirm the dataset version is available.
4. Ask agents questions grounded in that dataset.
5. Use traces to inspect the activity behind important answers.

## Freshness and readiness

Dataset pages should make readiness understandable. If a dataset is still processing, blocked, or unavailable, the product should show customer-safe status and next steps without exposing internal job orchestration details.

## Tenant isolation

Your dataset experience should be scoped to your tenant only. You should not be able to see other customers' datasets, lineage, or uploads.

## Stable help target

Use `/datasets/` for in-product help from dataset lists, dataset detail pages, freshness indicators, and upload lineage links.
