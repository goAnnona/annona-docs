# DEV delivery loop runbook

This is the canonical operator/developer runbook for the Annona DEV web UI delivery loop that is proven today: web UI PR gates, merge to `main`, DEV image publication, cross-repo infrastructure handoff, DEV deploy, smoke, and proof artifact capture.

This runbook is DEV-scoped. It does not describe or imply a production release path, production approvals, production SLAs, or production data handling.

## Scope and source of truth

| Area | Source of truth | Workflow | Responsibility |
| --- | --- | --- | --- |
| Web UI PR validation | `goAnnona/annona-webui` | [`PR Gates`](https://github.com/goAnnona/annona-webui/actions/workflows/pr-gates.yml) | Validate a proposed web UI change before merge. |
| Main-merge DEV image handoff | `goAnnona/annona-webui` | [`Dev Delivery`](https://github.com/goAnnona/annona-webui/actions/workflows/dev-delivery.yml) | Build and publish a DEV image, write release artifacts, and dispatch the infra repository. |
| DEV cluster deploy, smoke, and proof | `goAnnona/annona-customer-infra` | [`Deploy annona-webui DEV`](https://github.com/goAnnona/annona-customer-infra/actions/workflows/deploy-annona-webui.yml) | Resolve deploy inputs, apply Kubernetes resources, expose the DEV service, run smoke checks, and capture proof artifacts. |
| Canonical operator docs | `goAnnona/annona-docs` | [`Docs CI`](https://github.com/goAnnona/annona-docs/actions/workflows/docs-ci.yml) and [`Deploy Docs`](https://github.com/goAnnona/annona-docs/actions/workflows/docs-pages.yml) | Publish truthful DEV documentation and runbooks. |

## Repo boundary diagram

```text
Developer PR
  |
  v
goAnnona/annona-webui
  PR Gates
    - lint, typecheck, unit tests, build, performance budgets
    - runtime surface, dependency, audit, and secret checks
    - Playwright E2E proof, screenshot proof, visual review, container smoke
  |
  | merge to main only after PR gates are acceptable
  v
goAnnona/annona-webui
  Dev Delivery
    - build and publish annona-webui DEV image
    - write dev-release artifacts and dispatch payload
    - repository_dispatch event: deploy-annona-webui-dev
  |
  | cross-repo handoff, not a Kubernetes deploy in webui
  v
goAnnona/annona-customer-infra
  Deploy annona-webui DEV
    - resolve image, namespace, bridge, and integrated-proof inputs
    - apply DEV namespace, secrets, services, and deployments
    - wait for rollout and DEV load balancer
    - run smoke-dev, proof-dev, and optional integrated-proof-dev
  |
  v
DEV proof package
  - deploy metadata, smoke summary, browser screenshots/html, optional integrated storage/trace checks
```

Boundary rule: `annona-webui` owns application code, PR validation, image publication, and the dispatch request. `annona-customer-infra` owns DEV AWS/EKS access, Kubernetes manifests, deployment execution, live service exposure, smoke validation, and proof capture after deployment. Do not treat a green `Dev Delivery` run as proof that the cluster deployed successfully; it proves that the web UI repository published the DEV artifact and handed off to customer-infra.

## Developer loop before merge

1. Open a PR in `goAnnona/annona-webui`.
2. Wait for `PR Gates` to run on `pull_request`, or run it manually with `workflow_dispatch` when needed.
3. Review all `PR Gates` jobs and artifacts:
   - `Quality Core`: `npm ci`, `npm run lint`, `npm run typecheck`, `npm test`, `npm run build:ci`, and `npm run check:performance`; uploads `build-diagnostics-<run_id>-<attempt>`.
   - `Security And Runtime Surface`: `npm run check:runtime-surface`, `npm run check:security:audit`, and `npm run check:security:secrets`; uploads `security-diagnostics-<run_id>-<attempt>`.
   - `Dependency Review`: `npm run check:dependencies`; uploads `dependency-review-<run_id>-<attempt>`.
   - `E2E Proof And Screenshots`: installs Chromium and runs `npm run test:e2e`; uploads `screenshot-proof-<run_id>-<attempt>`, `playwright-report-<run_id>-<attempt>`, and `playwright-test-results-<run_id>-<attempt>`.
   - `Canonical Visual Review`: runs `npm run visual:review` when `OPENAI_API_KEY` is configured, otherwise runs `npm run visual:review:mock`; uploads `visual-review-output-<run_id>-<attempt>`.
   - `Deployable Container Smoke`: builds the Docker image shape and uploads `container-metadata-<run_id>-<attempt>`.
4. Treat the PR as merge-ready only when the required gates are green and any warnings or artifact findings are understood.

## Main-merge DEV handoff

After the web UI PR merges to `main`, `Dev Delivery` runs in `goAnnona/annona-webui` on `push` to `main`. It can also be started manually with `workflow_dispatch`.

`Dev Delivery` does the webui-owned part of delivery:

1. Rebuilds the application with `npm run build:ci` and enforces `npm run check:performance`.
2. Validates that the DEV delivery contract is configured: `AWS_DEPLOY_ROLE_ARN_DEV` and `ANNONA_INFRA_REPO_DISPATCH_TOKEN` must be present.
3. Assumes the DEV AWS role, logs in to Amazon ECR, and publishes the image as both:
   - a versioned tag: `annona-webui:dev-<short_sha>`
   - a floating tag: `annona-webui:dev-latest`
4. Writes `artifacts/dev-release/release-manifest.json` with the commit, image, floating tag, `channel: development`, and `productionPathDefined: false`.
5. Writes `artifacts/dev-release/infra-dispatch.json` with `event_type: deploy-annona-webui-dev` and a payload containing the image, namespace, source repository, source SHA, source run ID, source run URL, and source workflow.
6. Calls the GitHub repository dispatch API for `goAnnona/annona-customer-infra`.
7. Uploads `dev-release-<run_id>-<attempt>` and writes a summary that explicitly says the workflow publishes a DEV artifact only and that no production path is defined.

Operator handoff expectation: copy the `Dev Delivery` run URL and image tag into the delivery thread or ticket, then follow the matching `Deploy annona-webui DEV` run in `goAnnona/annona-customer-infra`.

## Customer-infra dispatch and deploy responsibilities

`Deploy annona-webui DEV` in `goAnnona/annona-customer-infra` accepts either:

- `repository_dispatch` with type `deploy-annona-webui-dev` from `Dev Delivery`; or
- manual `workflow_dispatch` with an explicit `image` and optional DEV inputs.

Customer-infra owns these deployment responsibilities:

1. Resolve the deploy image, namespace, data-lake bridge options, and integrated-proof options from the dispatch payload or manual inputs.
2. Configure DEV AWS credentials and update kubeconfig for the DEV EKS cluster.
3. Apply the DEV namespace.
4. If `data_lake_bridge_enabled` is true, validate the bridge contract, deploy the annona-data-lake submit bridge, and smoke `/healthz` plus `/v1/customer-status` through a port-forward.
5. Apply `annona-webui` Kubernetes secrets, including provider keys, the DEV demo password, and the optional data-lake token.
6. Deploy `annona-webui` with the requested image and injected data-lake submit/status URLs.
7. Wait for `kubectl rollout status deployment/annona-webui`.
8. Apply the DEV public service and wait for a load balancer hostname or IP.
9. Upload `annona-webui-dev-deploy-<run_id>-<attempt>` with deploy metadata and any bridge deploy artifacts.
10. On deploy failure, run `scripts/collect-k8s-diagnostics.sh` for cluster, deployment, service, pod, log, previous-log, and event diagnostics.

Manual dispatch inputs should stay DEV-specific. The workflow name, namespace defaults, EKS defaults, summary, and release manifest all describe DEV delivery; production is disabled in this loop.

## Smoke expectations

The `smoke-dev` job runs after `deploy-dev` succeeds in `Deploy annona-webui DEV`.

A passing smoke run proves the freshly deployed DEV service is reachable and serving the expected runtime surfaces:

- `GET /` returns HTML from the live DEV service and stores `root.html`.
- `GET /api/config?provider=openai` returns JSON and stores `api-config.json`.
- If the data-lake bridge was requested, `GET /api/customer-status` returns the expected bridge-backed customer snapshot shape and stores `customer-status.json`.
- If a DEV demo password is configured, `POST /api/auth/check` accepts it and stores `auth-check.json`; if no password is configured, that check is skipped rather than claimed.
- `smoke-summary.json` records status, checks, base URL, and timestamp.

The smoke artifact is `annona-webui-dev-smoke-<run_id>-<attempt>`.

## Proof expectations

The `proof-dev` job runs after `deploy-dev` and `smoke-dev`, as long as deployment succeeded. It captures browser proof from the DEV base URL:

- `01-landing.png` and `01-landing.html` for the landing page.
- `02-authenticated.png` and `02-authenticated.html` when a password gate is present and a DEV demo password is available.
- `body-snippet.txt` and `proof-summary.json` describing title, final URL, password-gate state, screenshots, notes, and any error.

The proof artifact is `annona-webui-dev-proof-<run_id>-<attempt>`.

The optional `integrated-proof-dev` job runs only when `integrated_proof_enabled` resolves to true. It validates customer proof contracts against DEV storage and tracing surfaces, including RAW landing data, ingestion manifests, optional BRONZE landing data, trace events, trace manifests, Athena trace row counts, Athena query results, and optional MLflow mirror checkpoints. It uploads `annona-webui-dev-integrated-proof-<run_id>-<attempt>` containing `integrated-proof-summary.json`.

## What counts as done

A DEV delivery is done when all of the following are true:

1. The web UI PR has acceptable `PR Gates` results and the PR is merged to `main`.
2. The `Dev Delivery` run for the merge commit succeeds and uploaded `dev-release-<run_id>-<attempt>`.
3. A corresponding `Deploy annona-webui DEV` run in `goAnnona/annona-customer-infra` uses the expected `annona-webui:dev-<short_sha>` image.
4. `deploy-dev` succeeds, the run summary lists the DEV cluster, namespace, image, and base URL, and the workflow uploaded `annona-webui-dev-deploy-<run_id>-<attempt>`.
5. `smoke-dev` succeeds and uploaded `annona-webui-dev-smoke-<run_id>-<attempt>`.
6. `proof-dev` succeeds and uploaded `annona-webui-dev-proof-<run_id>-<attempt>`.
7. If integrated proof was requested, `integrated-proof-dev` succeeds and uploaded `annona-webui-dev-integrated-proof-<run_id>-<attempt>`; if it was not requested, the delivery notes say integrated proof was disabled or not requested.
8. The delivery note links the webui `Dev Delivery` run, the customer-infra `Deploy annona-webui DEV` run, the image tag, the DEV base URL, and the smoke/proof artifact names.

Do not mark DEV delivery done based only on a successful PR gate, a pushed image, or a green webui `Dev Delivery` run. The cluster deploy, smoke, and proof signals live in `annona-customer-infra`.

## Troubleshooting

### `PR Gates` fails

- Start with the failed job name. `Quality Core` failures usually belong to app code, types, tests, build, or performance budgets in `annona-webui`.
- For `Security And Runtime Surface`, inspect `security-diagnostics-<run_id>-<attempt>` before changing runtime dependencies or secret patterns.
- For `E2E Proof And Screenshots` or `Canonical Visual Review`, compare screenshot proof, Playwright report, and visual review output. If `OPENAI_API_KEY` is absent, the visual review job intentionally uses the mock fallback and should not be represented as model-reviewed proof.
- For `Deployable Container Smoke`, inspect `container-metadata-<run_id>-<attempt>` and the Docker build logs before assuming the app can be deployed.

### `Dev Delivery` fails before dispatch

- If `Validate DEV delivery contract` fails, configure `AWS_DEPLOY_ROLE_ARN_DEV` or `ANNONA_INFRA_REPO_DISPATCH_TOKEN` in the webui repo/environment/organization as indicated by the workflow error.
- If ECR login or image push fails, verify the DEV AWS role and repository permissions; this is still a webui-side publication failure, not a customer-infra deploy failure.
- If dispatch fails, inspect `artifacts/dev-release/infra-dispatch.json` and confirm the token can call the repository dispatch API for `goAnnona/annona-customer-infra`.

### `Deploy annona-webui DEV` fails in deploy

- Confirm the run was triggered by `repository_dispatch` type `deploy-annona-webui-dev` or by manual `workflow_dispatch` with a fully qualified `image`.
- Check `Resolve deploy inputs` first for missing image, bad namespace, missing data-lake bridge image/contract, or missing integrated-proof contract.
- For AWS/EKS failures, check the DEV role, `AWS_REGION`, `EKS_CLUSTER_DEV`, and kubeconfig output in the run log.
- For rollout failures, inspect the automatic diagnostics from `scripts/collect-k8s-diagnostics.sh`, then review pod events and current/previous container logs.
- For load balancer failures, inspect the public service named `annona-webui-public` in the target namespace and confirm a hostname or IP was assigned.

### `smoke-dev` fails

- Use the deploy summary base URL and smoke artifact `smoke-summary.json` to identify the failed endpoint.
- `GET /` or HTML failures usually indicate service exposure, DNS/load balancer readiness, or pod startup issues.
- `/api/config?provider=openai` failures usually indicate runtime configuration or application API issues.
- `/api/customer-status` failures only apply when the data-lake bridge was requested; verify bridge deployment, bridge `/healthz`, bridge `/v1/customer-status`, injected URLs, and data-lake token configuration.
- `POST /api/auth/check` failures usually indicate a wrong or missing DEV demo password secret.

### `proof-dev` or `integrated-proof-dev` fails

- For `proof-dev`, inspect `proof-summary.json`, screenshots, and HTML captures to see whether the app loaded, whether a password gate blocked proof, or whether browser capture failed.
- If the password gate is present but `PROOF_DEMO_PASSWORD` was unavailable, the proof captures a note rather than authenticated evidence; decide whether that is acceptable for the delivery.
- For `integrated-proof-dev`, inspect `integrated-proof-summary.json` for the first failed storage, trace, Athena, or MLflow check.
- Empty S3 prefixes, missing Athena rows, or missing query-result objects mean the integrated proof has not demonstrated live DEV ingestion/tracing evidence yet.
- If integrated proof was not enabled, do not claim integrated storage/tracing proof for the delivery.
