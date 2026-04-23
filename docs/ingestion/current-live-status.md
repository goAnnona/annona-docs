# Current live ingestion status

## Status

**Not ready end to end yet.**

## What is live now

- onboarding workspace is visible in the web UI
- customer selection works
- accepted bundle file types are shown
- file selection works in the browser

## Current blocker

The live submit step is disabled until the web runtime is configured with `ANNONA_DATA_LAKE_SUBMIT_URL`.

That means the current experience can be demonstrated up to file selection, but not through a full live intake handoff yet.

## Documentation follow-up

Once the live bridge is wired, this page should be expanded with:

- end-to-end submit steps
- screenshots of a successful intake
- job status interpretation
- RAW / BRONZE / DuckDB outcomes
