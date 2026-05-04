# Annona user guide

Annona is organized into clear customer destinations so day-to-day work stays understandable and tenant-scoped.

## Your customer tenant

Your organization uses its own Annona tenant, typically at a customer-specific subdomain such as `<your-company>.annona...`.

Inside your tenant, navigation should guide you through a small number of durable jobs:

- [Home](home.md) for welcome, readiness, and next steps
- [Onboarding](onboarding.md) for first setup and activation
- [Uploads](uploads.md) for bringing in new data
- [Agents](agents.md) for asking questions and reviewing grounded recommendations
- [Datasets](datasets.md) for understanding available data and freshness
- [Traces](traces.md) for run history, evidence, and troubleshooting
- [Help](help.md) for docs and support links

## Customer workspace model

Your tenant should feel like your own workspace.

That means you should be able to tell:

- which organization and tenant you are in
- which datasets and uploads belong to your organization
- which traces belong to your own activity
- where to get help without being dropped into internal Annona tooling

See [Customer workspace](help/customer-workspace.md) for the workspace boundary and what customer users should expect.

## What you should not see

Customer tenants should not expose internal admin workflows such as:

- creating or provisioning other customer tenants
- editing global schemas or platform configuration
- testing another customer's agents
- viewing platform-wide traces or traces outside your tenant
- using operator-only controls that are not part of the customer workflow

## Help model

Whenever the product offers contextual help, the destination should be a stable page in this docs site. That keeps the customer experience consistent between in-product guidance and the help center.

See [Stable help-link targets](help-link-targets.md) for the named slugs product teams can use.
