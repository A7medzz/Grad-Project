# Stage 09 — Reporting and Simplified Public Experience

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Select the first export format and required report sections.
- [ ] Define whether simplified public use requires an account and set usage limits.
- [ ] Define export retention, authorized downloads, and source-content restrictions.
- [ ] Decide whether audience analysis appears in the simplified public interface; do not expose it by default.

## Build

### 1. Complete workspace navigation and history

- [ ] Integrate saved documents, claims, evidence, comparisons, reactions, notes, and review states.
- [ ] Make changed inputs, stale analyses, unavailable sources, and superseded conclusions visible.
- [ ] Preserve work across sessions and prevent model reruns from overwriting user edits.
- [ ] Keep source-specific refresh/deletion conditions effective across all views.

### 2. Implement report assembly

- [ ] Let the user select authorized sections and the report language.
- [ ] Include claim wording/revision, evidence citations, historical checks, provisional assessments, and reviewer conclusions.
- [ ] Include comparisons/reaction analyses only when present and selected.
- [ ] Include sample denominators, collection window, coverage, uncertainty, and review status where applicable.
- [ ] Check source quotation, storage, attribution, and export restrictions before including content.
- [ ] Create an explicit report snapshot with input versions; do not imply it remains current after edits.
- [ ] Implement preview and one selected export format before adding more formats.
- [ ] Support readable Arabic/English formatting, links, page references, and restricted downloads.

### 3. Implement the simplified public workflow

- [ ] Apply the chosen anonymous/authenticated access and usage-limit policy.
- [ ] Accept a direct claim, article URL/text, or text-based PDF through existing intake services.
- [ ] Offer claim selection only when needed.
- [ ] Present prior checks or evidence-backed explanations with accessible sources and dates.
- [ ] Explain AI/review status, insufficient evidence, unavailable data, and extraction limits plainly.
- [ ] Reuse the scoped chatbot for explanations where enabled.
- [ ] Keep submissions private by default; public access is not automatic public publication.
- [ ] Do not expose audience-reaction analysis in this experience until its presentation is explicitly decided.

### 4. Harden privacy and output delivery

- [ ] Recheck authorization during report assembly and download, not only when the job starts.
- [ ] Avoid exposing raw storage paths or making private export URLs permanently public.
- [ ] Sanitize rendered source text/links and generated report content.
- [ ] Apply retention/deletion and source revocation rules to cached exports and temporary files.
- [ ] Test request limits, abuse controls, and actionable failure messages.

## Data, API, and Screen Checklist

- [ ] Store report snapshots, input versions, selected sections, language, format, access scope, and export-job status.
- [ ] Implement report generation, for example `POST /investigations/{investigation_id}/reports`.
- [ ] Implement authorized preview/download, for example `GET /reports/{report_id}`.
- [ ] Connect public submission/results screens to the same scoped investigation services.
- [ ] Connect workspace history, report section selection, preview, export status, and simplified source/explanation views.

## Check

- [ ] Exported citations resolve to the intended source/version or explicitly state unavailable material.
- [ ] AI suggestions, historical assessments, and human conclusions cannot be mistaken for one another.
- [ ] Arabic/English report previews and exports remain readable with mixed-direction citations.
- [ ] Reaction sections include sample counts and limitations rather than population claims.
- [ ] Source restrictions exclude prohibited content from report files and cached previews.
- [ ] Another user cannot access a report through its identifier, job, preview, or download.
- [ ] Public users can complete the simplified workflow without seeing private investigations.
- [ ] Test comprehension and task completion with representative professional/public users.

- [ ] Complete the professional flow: reopen findings → select report sections → preview → export with citations and review status.
- [ ] Complete the simplified public flow: submit a claim/document → select a claim if needed → inspect an explanation and its sources.

**Next:** [Stage 10 — Validation and deployment](10-validation-and-deployment.md)
