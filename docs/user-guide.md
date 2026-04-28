# Annona user guide

Annona is organized into separate surfaces so customer work stays focused, understandable, and tenant-scoped.

## Your customer tenant

Your organization uses its own Annona tenant, typically at a customer-specific subdomain such as `<your-company>.annona...`.

Inside your tenant, navigation should guide you through a small number of clear jobs:

- [Home](home.md) for welcome, onboarding state, and service status
- [Onboarding](onboarding.md) for the first setup path after a new tenant is created
- [Agents](agents.md) for asking questions and reviewing grounded recommendations
- [Datasets](datasets.md) for managing the datasets available to your organization
- [Uploads](uploads.md) for bringing new data into Annona
- [Traces](traces.md) for viewing the activity history for your own runs
- [Help](help.md) for documentation and support links

## Current route model

The current DEV product split uses customer-facing routes for `/`, `/agents`, `/datasets`, `/uploads`, `/traces`, and `/help`. The `/jobs` route may remain as an uploads compatibility alias while product links move toward `/uploads`.

The long-term product framing is a dedicated customer tenant surface separate from Annona's internal admin surface. Customer help should document the tenant experience, not internal setup or operations.

## What you should not see

Customer tenants should not expose internal admin workflows such as:

- creating or provisioning other customer tenancies
- editing global schemas or platform configuration
- testing another customer's agents
- viewing platform-wide traces or traces outside your tenant
- choosing internal model controls that are not part of your customer workflow

## Help model

Whenever the product offers contextual help, the destination should be a stable page in this docs site. That keeps the customer experience consistent between in-product guidance and the help center.

See [Stable help-link targets](help-link-targets.md) for the named slugs product teams can use.
