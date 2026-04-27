# Annona user guide

Annona is organized into separate surfaces so that customer work stays focused and understandable.

## Your customer tenant

Your organization uses its own Annona tenant, typically at a customer-specific subdomain such as `<your-company>.annona...`.

Inside your tenant, the navigation should guide you through a small number of clear jobs:

- **Home** for welcome, onboarding, and status
- **Agents** for asking questions and comparing grounded versus generic answers when available
- **Datasets** for managing the datasets available to your organization
- **Uploads** for bringing new data into Annona
- **Traces** for viewing the activity history for your own runs
- **Help** for documentation and support links

## What you should not see

Customer tenants should not expose internal admin workflows such as:

- creating other customer tenancies
- editing global schemas or platform configuration
- testing another customer's agents
- viewing traces outside your own tenant

## Help model

Whenever the product offers contextual help, the destination should be a page in this docs site. That keeps the customer experience consistent between in-product guidance and the help center.
