# Project Build Blueprint — Master Checklist

**Project:** Bilingual Computational Journalism Platform  
**Scope:** [Project README](../../README.md)  
**Progress:** Not started

Use this as the list of things the project needs. Each linked stage contains the detailed checklist for designing, building, and checking that part. Check off an item when its stated work is done; unchecked choices are not finalized decisions.

The order follows technical dependencies, not a rigid schedule. Build the foundation, intake, and search first. Comparison and reaction analysis are separate branches; chat and reporting can start once the investigation services they need exist.

## 01. [Application Design](stages/01-technical-design.md)

- [ ] Choose the frontend, backend, database, storage, and background-job tools.
- [ ] Define the project folders and how the web app, API, database, and worker connect.
- [ ] Design the initial database tables, relationships, and private-access rules.
- [ ] Define the first API requests, responses, and processing states.
- [ ] Sketch the investigation, submission, document-viewer, and bilingual navigation screens.

Starting candidates—not selected technologies—are Next.js/TypeScript, FastAPI/Python, PostgreSQL with pgvector and keyword search, private file storage, and a queue/worker.

## 02. [Application Foundation](stages/02-application-foundation.md)

- [ ] Set up the frontend, backend, database migrations, and local startup commands.
- [ ] Implement authentication and private investigation access.
- [ ] Build investigation creation, listing, editing, and deletion.
- [ ] Build the Arabic/English application shell and RTL layout.
- [ ] Implement background jobs, status/error handling, and basic automated tests.

## 03. [Content Intake and Provenance](stages/03-content-intake.md)

- [ ] Accept pasted text, permitted article URLs, and text-based PDFs.
- [ ] Extract text while preserving original content, source metadata, and PDF pages.
- [ ] Store private files, document versions, and retrievable passages.
- [ ] Add selected core-source connectors and duplicate detection.
- [ ] Build submission/status screens and the original/extracted document viewer.
- [ ] Handle inaccessible URLs, unsafe inputs, unsupported PDFs, and content deletion.

## 04. [Bilingual Search and Previous Checks](stages/04-search-and-fact-checks.md)

- [ ] Select multilingual embeddings and implement passage indexing.
- [ ] Implement keyword and semantic retrieval with source/date/language filters.
- [ ] Connect selected news, primary-document, and historical fact-check sources.
- [ ] Match previous checks while exposing date/entity/quantity/context differences.
- [ ] Build search results, cited excerpts, and previous-check views.
- [ ] Test all four Arabic/English query–document language combinations.

## 05. [Claim Investigation and Human Review](stages/05-claim-investigation.md)

- [ ] Generate source-grounded summaries and editable factual claims.
- [ ] Support direct claim entry and preserve claim revisions.
- [ ] Retrieve evidence and identify supporting, contradicting, and irrelevant passages.
- [ ] Generate provisional explanations with citations, conflicts, and evidence gaps.
- [ ] Support explicit insufficient-evidence outcomes.
- [ ] Save notes and reviewer conclusions separately from AI suggestions and historical verdicts.

## 06. [Same-Event Coverage Comparison](stages/06-coverage-comparison.md)

- [ ] Let users select articles and confirm the event being compared.
- [ ] Align corresponding facts and claims across Arabic/English articles.
- [ ] Show shared reporting, differing claims, and differences in emphasis with excerpts.
- [ ] Build side-by-side inspection and save the comparison.
- [ ] Flag different events or inadequate material instead of forcing a comparison.

## 07. [Audience-Reaction Analysis](stages/07-audience-reactions.md)

- [ ] Connect at least one permitted source of automatically retrieved public reactions.
- [ ] Let users specify the decision/issue, sources, and collection window.
- [ ] Collect relevant reactions and available context; filter unrelated material and duplicates.
- [ ] Implement sentiment and decision-relative support/opposition as separate analyses.
- [ ] Summarize supporting/opposing reasons with linked examples.
- [ ] Show sample counts, distributions, source coverage, uncertainty, and unavailable-data states.

Check reaction-source access early alongside core development. Social search supplements the platform; its failure must not block the main investigation.

## 08. [Bilingual Investigation Chatbot](stages/08-investigation-chatbot.md)

- [ ] Build Arabic/English conversations scoped to accessible investigation material.
- [ ] Retrieve relevant documents, PDF pages, evidence, and saved analysis.
- [ ] Answer follow-up questions with citations and clear uncertainty.
- [ ] Use only enabled search services when more material is needed.
- [ ] Prevent source instructions from changing access scope or silently editing user records.

## 09. [Reporting and Simplified Public Experience](stages/09-reporting-and-public-experience.md)

- [ ] Complete navigation between saved material, findings, notes, and review history.
- [ ] Build report section selection, preview, and one export format.
- [ ] Include citations, limitations, review status, and sample descriptions where relevant.
- [ ] Build simplified claim/URL/text/PDF submission and explanation screens.
- [ ] Keep private uploads, investigations, and exports inaccessible to other users.

## 10. [Testing and Deployment](stages/10-validation-and-deployment.md)

- [ ] Test the complete journalist and simplified public workflows.
- [ ] Measure retrieval, extraction, assessment, comparison, reaction, and chatbot quality.
- [ ] Test access control, unsafe inputs, missing sources, retries, and provider failures.
- [ ] Deploy the frontend, API, database, worker, models, and private storage.
- [ ] Configure secrets, network security, logs, backups, and recovery.
- [ ] Document setup, startup, test commands, deployed configuration, and known limitations.

## Shared Requirements — Apply While Building Each Feature

- [ ] Support Arabic, English, RTL, and mixed-direction text in the interface.
- [ ] Preserve original text, source URLs, known publication dates, retrieval times, and page/passage citations.
- [ ] Enforce access restrictions in APIs, retrieval, workers, chat, files, and exports.
- [ ] Validate files/URLs, secure sessions/secrets, and sanitize displayed content.
- [ ] Keep uploaded material private and check which external services may receive it.
- [ ] Track input versions and make outdated analysis visible after material edits.
- [ ] Keep AI output, previous-check assessments, and human conclusions distinct.
- [ ] Handle loading, empty results, unsupported inputs, partial failures, and retry states.
- [ ] Apply source processing/storage/export conditions and delete or refresh dependent content when required.
- [ ] Show insufficient evidence and unavailable reaction data without inventing conclusions.
- [ ] Describe reactions as a collected sample, never as population opinion or proof of truth.
- [ ] Build tests with each feature, not only at the final stage.

## Research Work — Alongside Feature Development

- [ ] Choose one research improvement from the project scope and define its baseline.
- [ ] Prepare labelled evaluation cases and annotation rules.
- [ ] Keep related claims, translations, threads, and events together when splitting data.
- [ ] Separate development and held-out tests; do not tune on final test results.
- [ ] Save dataset/model versions, configuration, prompts, and evaluation results.
- [ ] Compare the improvement with the baseline and report errors, latency, cost, and limitations.

Research choices do not block building the application foundation.

## Optional Future Work — Not Required for the Main Build

- [ ] User-submitted social posts.
- [ ] Framing and persuasion-technique analysis.
- [ ] Event timelines.
- [ ] Topic monitoring and recurring-claim alerts.
- [ ] Team collaboration and review assignments.
- [ ] Screenshot text extraction and scanned-PDF OCR.
- [ ] Browser extension.
