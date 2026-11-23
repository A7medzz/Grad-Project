# Stage 10 — System Validation and Deployment

[Master checklist](../README.md)

## Test the Complete Application

### 1. Validate complete user journeys

- [ ] Test direct claims and article/text/PDF submissions through search, investigation, review, and report export.
- [ ] Test independent coverage comparison and audience-analysis branches.
- [ ] Test bilingual chat against documents, evidence, and available saved analyses.
- [ ] Validate the simplified public experience and private professional workspaces.
- [ ] Include Arabic/RTL, English, mixed-direction content, keyboard access, and readable error states.

### 2. Run component quality tests

- [ ] Freeze final model/configuration versions before held-out evaluation.
- [ ] Run retrieval, historical matching, extraction, summaries, evidence interpretation, and sufficiency evaluations.
- [ ] Evaluate coverage comparisons, reaction relevance/classification/reasons, chat citations, and report preservation.
- [ ] Report retrieval language directions, Arabic/English reaction results, dialect/target weaknesses, and error analysis.
- [ ] Run the research baseline/improvement comparison on identical splits if an improvement has been selected; this experiment does not block testing unrelated features.
- [ ] Record failure examples and, for the research comparison, useful ablations and annotation disagreements.
- [ ] Report quality together with latency, memory/compute, and cost; do not claim unmeasured success.

### 3. Test security, privacy, and adverse conditions

- [ ] Exercise cross-user access attempts at API, search, file, worker, chat, and report layers.
- [ ] Test unsafe URLs/redirects, oversized/malformed files, rendered-content injection, and prompt injection.
- [ ] Verify session/secret configuration and dependency vulnerability review.
- [ ] Test disabled connectors, quotas, unavailable replies, missing evidence, provider timeouts, and model failures.
- [ ] Verify idempotent retries, cancellation, concurrent edits, stale outputs, and restart recovery.
- [ ] Test source refresh/revocation and user deletion across database, files, indexes, caches, outputs, and backup policy.
- [ ] Confirm that missing social/reaction data never prevents the core investigation path.

## Deploy

- [ ] Select the deployment target and configure service runtimes, model execution, storage, and network connections.
- [ ] Package the selected services and document environment variables without secrets.
- [ ] Apply production migrations and safe configuration for database, storage, queue, API, and frontend.
- [ ] Enable TLS, restricted network access, appropriate request/rate limits, and authorized storage.
- [ ] Configure health checks, job/error logs, and resource limits; add spending caps/alerts if paid services are enabled.
- [ ] Configure backups and retention for the database and private files.
- [ ] Test restoring a backup and reverting a failed application/schema deployment.
- [ ] Separate synthetic/demo fixtures from private or restricted real data.

## Make the Application Reproducible

- [ ] Write reproducible setup, startup, seed, test, evaluation, and deployment instructions.
- [ ] Document model/source versions, licenses, attribution, restrictions, and known unavailable integrations.
- [ ] Add clearly labelled test/demo fixtures for repeatable checks when live services are unavailable.
- [ ] Update the project README with actual setup, working features, limitations, and test/evaluation commands.

## Check the Deployed System

- [ ] End-to-end and security tests pass; quality/latency results identify which configured targets are met.
- [ ] Each main feature has a reproducible working check or an explicit failure/unavailable state.
- [ ] A blocked reaction source is not reported as a validated audience-analysis integration.
- [ ] A fresh deployment can be reproduced from the documentation and required authorized credentials.
- [ ] Backup restoration and critical failure recovery are demonstrated.
- [ ] No unsupported claim of automated truth detection, population sentiment, or research novelty appears in the UI or report.
- [ ] Known failures, unavailable integrations, and limitations are documented.

- [ ] Complete the deployed flow: investigate a claim → compare coverage → analyse available reactions → ask cited follow-ups → export a reviewed report.
- [ ] Repeat the core investigation with social search disabled and demonstrate an insufficient-evidence outcome.
