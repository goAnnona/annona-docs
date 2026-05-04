# Stable help-link targets

Product help links should use stable customer docs slugs that match the customer tenant IA. These URLs are intentionally named so they can remain stable while implementation details evolve.

## Target map

| Customer product area | Stable slug | Use for |
| --- | --- | --- |
| Home | `/home/` | tenant welcome, readiness, status, next steps |
| Onboarding | `/onboarding/` | first-run setup, setup tasks, readiness banners |
| Uploads | `/uploads/` | upload wizard, validation, processing status, `/jobs` alias help |
| Agents | `/agents/` | agent lists, prompts, grounded answers, evidence |
| Datasets | `/datasets/` | dataset lists, details, freshness, upload lineage |
| Traces | `/traces/` | run history, trace detail, evidence, troubleshooting |
| Customer workspace | `/help/customer-workspace/` | tenant scope, workspace orientation, customer-safe navigation |
| Account and access | `/account/access/` | sign-in, invitations, access-denied states, account settings |
| Help | `/help/` | global help menu, support entry points, documentation launchers |

## Linking rules

- Link to the closest workflow page rather than the docs home page when context is known.
- Use customer-safe language and avoid internal repository, admin, deployment, or operator links.
- Prefer these stable slugs over implementation-specific route names when they differ.
- Keep `/uploads/` as the docs target even if a product environment still exposes `/jobs` as an upload-processing alias.
- Use `/help/customer-workspace/` when the product needs to explain tenant scope, navigation, or customer-workspace expectations.

## Public docs boundary

The public customer docs site should not publish internal admin guides, operator runbooks, platform deployment notes, private proof artifacts, or architecture decisions. Those materials belong in Annona's internal documentation systems.
