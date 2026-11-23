# Stage 07 — Audience-Reaction Analysis

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Test at least one permitted news-comment or social connector for automatic search, useful Arabic/English reactions, and available context.
- [ ] Check retrieval, processing, storage, refresh/deletion, and export conditions separately.
- [ ] Select sentiment and target-conditioned stance models and define their input/output formats.
- [ ] Set collection caps, sample-size rules, and handling for mixed/uncertain predictions.
- [ ] Define whether displayed aggregates use model labels, reviewed labels, or both.

Run source-access checks alongside core development; unavailable reactions must not block claim investigation. No social provider is selected yet. Use free access first; paid access requires separate approval.

## Build

### 1. Define and preserve the target

- [ ] Ask the user to select the decision/issue and collection window explicitly.
- [ ] Store target wording, context, entities, language, and version.
- [ ] Keep decision-relative stance separate from whether an announcement is genuine or a claim is correct.
- [ ] Define support/oppose/no-clear-position and a separate uncertain/abstained processing state.
- [ ] Define mixed sentiment, conditional support, sarcasm, missing context, and quoted disagreement rules.

### 2. Implement bounded automatic collection

- [ ] Generate Arabic/English queries and useful spelling/entity variants without changing the target.
- [ ] Search enabled, permitted source APIs/connectors with explicit result/time/resource caps.
- [ ] Store collection query, dates, source IDs/links, parent/quoted context, and retrieval outcomes.
- [ ] Retrieve replies/context only where separately permitted and available.
- [ ] Mark missing replies, inaccessible context, rate limits, and partial failures.
- [ ] Identify unrelated material and duplicate/reposted content; retain exclusion reasons.
- [ ] Do not use optional user-submitted social posts as a replacement for automatic discovery.

### 3. Implement and test sentiment and stance

- [ ] Evaluate candidate multilingual and Arabic sentiment checkpoints against actual collected text.
- [ ] Build a documented target-conditioned stance baseline; do not mistake a pretrained encoder for a trained stance classifier.
- [ ] Label a small test collection to check both classifiers; use independent annotation and resolve disagreements where feasible.
- [ ] Evaluate unfamiliar targets, dialects, code-switching, Arabizi, negation, quotation, sarcasm, and conditional support.
- [ ] Keep original-language text; translation may be a comparison baseline, not the only Arabic processing path.
- [ ] Record model revisions, preprocessing, target version, uncertainty, and reviewer corrections.

### 4. Summarize reasons and aggregate honestly

- [ ] Link each supporting/opposing reason to reaction examples.
- [ ] Define grouping and counting rules so repeated copies do not imply independent recurrence.
- [ ] Allow multiple reasons per reaction; explain when reason percentages do not sum to 100%.
- [ ] Show retrieved, excluded, deduplicated, analysed, uncertain, and classified counts.
- [ ] State denominators separately for sentiment, stance, and reason summaries.
- [ ] Show collection dates, sources, unavailable context, and sample limitations.
- [ ] Define minimum usable-sample/context rules and show an inadequate-data state when they are not met.

### 5. Add inspection, review, and persistence

- [ ] Build target/source/window controls, job status, distributions, reasons, and original-reaction views.
- [ ] Separate low confidence from a prediction of no clear position.
- [ ] Make reviewer overrides identifiable and decide whether displayed aggregates use reviewed or model labels.
- [ ] Save sample/version/configuration links for chat and reporting.
- [ ] Apply source refresh/deletion rules to raw text, examples, derived results, caches, and exports.

## Data, API, and Screen Checklist

- [ ] Store decision targets/versions, collection runs, reactions/context, duplicates, exclusions, predictions, reason groups, and review changes.
- [ ] Implement collection submission, for example `POST /investigations/{investigation_id}/reaction-collections`.
- [ ] Implement authorized sample/result retrieval, for example `GET /reaction-collections/{collection_id}`.
- [ ] Expose reaction inspection and reviewer corrections without losing original model outputs.
- [ ] Connect the target form, source/coverage status, sample dashboard, reaction inspector, and review controls.

## Check

- [ ] Negative sentiment can coexist with support; questions need not imply opposition.
- [ ] Irrelevant targets, quoted rumors, copies, missing parents, and unavailable replies are handled explicitly.
- [ ] Counts and denominators reconcile, including exclusions and uncertain predictions.
- [ ] Reasons are supported by examples and do not exaggerate prevalence or invent consensus.
- [ ] Report sentiment/stance per-class metrics and macro-F1, annotation agreement, retrieval relevance, latency, and cost.
- [ ] Split related threads, translations, and targets appropriately; evaluate unseen decisions.
- [ ] Empty or unavailable reaction sources leave core investigation operational.
- [ ] No screen presents sample agreement as truth or general public opinion.
- [ ] Test the chosen sample-size and uncertainty rules, including too few reactions and ambiguous predictions.

- [ ] Complete the working flow: specify a decision → collect reactions → inspect sentiment/stance and reasons → review/save sample findings.

**Next:** [Stage 08 — Bilingual investigation chatbot](08-investigation-chatbot.md)
