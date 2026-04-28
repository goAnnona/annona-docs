# Account and access

Annona uses separate customer and internal admin surfaces so customer data and workflows stay tenant-scoped.

## Customer access

Customers sign in to their own tenant and should only receive access to their organization's data and tools. Your tenant may use invitations, organization-managed access, or another configured sign-in flow.

## Invitations and sign-in

If you cannot access your tenant:

- confirm you are using the customer tenant URL for your organization
- check whether your invitation has expired or was sent to another email address
- ask your Annona contact or organization admin to confirm your access
- use Help for support if sign-in still fails

## Tenant boundaries

Inside your tenant, you should only see customer-facing destinations such as Home, Onboarding, Agents, Datasets, Uploads, Traces, and Help. Internal admin surfaces are separate and are not part of the customer help center.

## Future identity direction

Annona is designed to support stronger social and enterprise identity options over time, including common enterprise identity providers. The customer access model should stay stable even as underlying login providers evolve.

## Stable help target

Use `/account/access/` for in-product help from sign-in, invitation, access-denied, tenant-switching, and account-management states.
