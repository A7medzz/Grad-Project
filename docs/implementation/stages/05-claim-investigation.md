# Stage 05 — Claim Investigation and Human Review

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Define assessment labels and how insufficient/conflicting evidence is represented.
- [ ] Select generation and evidence-interpretation models with structured, cited output formats.
- [ ] Define who can record a reviewer conclusion and how edits/history are stored.
- [ ] Define how claim changes invalidate previous evidence searches and assessments.

## Build

### 1. Add summaries and editable claims

- [ ] Generate summaries grounded in selected passages, with source references.
- [ ] Propose factual claims linked to original passages; distinguish opinions and unverifiable wording.
- [ ] Let users create a direct claim, select extracted claims, and edit their wording.
- [ ] Preserve claim revisions and distinguish the submitted text from the current investigation target.
- [ ] Mark previous analysis stale when a material claim edit changes its meaning or scope.

### 2. Implement evidence investigation

- [ ] Retrieve prior checks without making a prior match mandatory.
- [ ] Generate editable investigation questions when useful.
- [ ] Search for candidate evidence through stage 04, including source-language variations.
- [ ] Let users attach or reject evidence and inspect its full permitted context.
- [ ] Analyse relevance and support/contradiction separately from source reliability and final assessment.
- [ ] Detect material time/entity/quantity differences and avoid counting copied reporting as independent confirmation.

### 3. Implement provisional assessments

- [ ] Apply the agreed assessment-label and evidence-sufficiency protocol.
- [ ] Generate explanations tied to specific evidence passages and unanswered questions.
- [ ] Check citation existence and version binding programmatically; assess semantic support through evaluation and review.
- [ ] Handle conflicting, outdated, missing, and inadequate evidence explicitly.
- [ ] Keep model output provisional; do not expose unsupported truth percentages.
- [ ] Record input versions, retrieval configuration, model revision, prompt/configuration, and costs where available.

### 4. Build reviewer persistence and UI

- [ ] Provide claim editing, evidence panels, gap lists, analysis status, notes, and review controls.
- [ ] Save AI suggestions separately from original historical verdicts and reviewer conclusions.
- [ ] Record reviewer identity, timestamps, rationale, and superseded decisions.
- [ ] Require user action to change conclusions; rerunning a model must not overwrite them.
- [ ] Reopen a saved investigation with citations, notes, and review state intact.

## Research Checklist — Alongside the Working Feature

- [ ] Freeze the baseline workflow before adding the selected research improvement.
- [ ] Implement the improvement behind a comparable configuration.
- [ ] Compare on the same development cases, including hard context mismatches and insufficient evidence.
- [ ] Preserve held-out test data for stage 10; do not claim novelty or improvement without evidence.

## Data, API, and Screen Checklist

- [ ] Store claim revisions, investigation questions, evidence links, analysis runs, notes, and reviewer conclusions.
- [ ] Expose summary and claim-extraction jobs for selected documents.
- [ ] Implement direct/extracted claim creation, for example `POST /investigations/{investigation_id}/claims`.
- [ ] Implement revision-protected claim editing, for example `PATCH /claims/{claim_id}`.
- [ ] Expose evidence-retrieval and assessment jobs for a specified claim revision.
- [ ] Implement evidence selection, notes, reviewer conclusions, and review-history access.
- [ ] Connect the claim workspace, evidence viewer, assessment/gaps panel, and review history.

## Check

- [ ] Direct-claim entry works without requiring an article upload.
- [ ] Summaries and claims are faithful to cited source material.
- [ ] Claim edits preserve history and invalidate incompatible analyses.
- [ ] Evidence interpretation handles support, contradiction, irrelevant passages, outdated sources, and copies.
- [ ] No existing check and insufficient evidence never automatically yield a false verdict.
- [ ] Reviewer conclusions remain distinct and survive model reruns, restarts, and concurrent edits.
- [ ] Record faithfulness, evidence adequacy, classification results where applicable, unsupported assessments, and unnecessary abstentions.
- [ ] Check assessment quality on labelled cases as well as testing that the workflow saves and returns results.

- [ ] Complete the working flow: select/edit a claim → inspect evidence and gaps → read the cited provisional assessment → save a reviewer conclusion.

**Next:** [Stage 06 — Coverage comparison](06-coverage-comparison.md); chatbot and reporting work can also begin against this foundation.
