# Stage 03 — Content Intake and Provenance

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Select article-text and text-based PDF extraction libraries.
- [ ] Set text length, file size, page count, timeout, and supported-layout limits.
- [ ] Choose private file storage and how authorized users open original files.
- [ ] Define which content each connector may store, refresh, delete, and display.

## Build

### 1. Add document and source records

- [ ] Store source role, origin, visibility, canonical URL, language, known publication date, and retrieval time.
- [ ] Add document versions, files, extraction outcomes, passages, and source-access restrictions.
- [ ] Preserve unknown metadata explicitly instead of inventing dates or publishers.
- [ ] Distinguish user submissions from retrieved original content.

### 2. Implement safe submission

- [ ] Build text submission with size and encoding limits.
- [ ] Build URL intake with permitted schemes, network restrictions, timeouts, response limits, and redirect validation.
- [ ] Block loopback, private, link-local, metadata-service, and other forbidden destinations, including after DNS resolution and redirects.
- [ ] Build private PDF upload with size/page limits, content validation, generated storage names, and constrained parser execution.
- [ ] Provide accessible file/job status without exposing storage paths or another user's files.

### 3. Extract and preserve references

- [ ] Extract article text without claiming inaccessible/paywalled content was retrieved.
- [ ] Extract text-based PDF pages and identify empty or incomplete extraction.
- [ ] Report scanned/image-only PDFs as unsupported initially; do not silently add OCR.
- [ ] Preserve original text alongside search-normalized text.
- [ ] Create passages with stable version/page/offset references.
- [ ] Build a viewer that opens a cited passage or PDF page in its source context.

### 4. Add shared connectors and preparation

- [ ] Implement a connector contract for discovery, fetch, capabilities, errors, quotas, and source conditions.
- [ ] Add the selected core connectors; route newly fetched material through the same intake process.
- [ ] Detect canonical duplicates, exact copies, and candidate near-duplicates without discarding provenance.
- [ ] Prepare passages for stage 04 indexing through an explicit extraction/indexing state.
- [ ] Preserve Arabic wording, entities, dates, quantities, and negation.

### 5. Implement lifecycle handling

- [ ] Use idempotent jobs and clear partial-failure outcomes.
- [ ] Make new source versions distinguishable from old ones; keep citations version-bound.
- [ ] Expose document-version changes so later search/analysis modules can invalidate stale outputs.
- [ ] Implement file/passage deletion and source refresh now; extend the same behavior to indexes, caches, and analyses when they are added.
- [ ] Retain only a minimal unavailable-source marker when required; do not retain prohibited content through history.

## API and Screen Checklist

- [ ] Implement text/URL/file submission, for example `POST /investigations/{investigation_id}/documents`.
- [ ] Implement document metadata and authorized content/file access, for example `GET /documents/{document_id}`.
- [ ] Implement passage retrieval, for example `GET /documents/{document_id}/passages`.
- [ ] Connect the submission panel, processing status, original/extracted-content viewer, and source metadata panel.

## Check

- [ ] Representative Arabic/English text, articles, and text-based PDFs produce inspectable content.
- [ ] PDF citations resolve to the correct original page.
- [ ] Broken URLs, blocked destinations, redirects, huge files, malformed PDFs, and extraction failures have explicit outcomes.
- [ ] Scanned documents are not presented as successfully extracted.
- [ ] Repeated submissions/retries do not create misleading independent-source counts.
- [ ] Original wording and normalized search text remain distinguishable.
- [ ] Private files and passages are inaccessible across users, including through guessed URLs and jobs.
- [ ] Deletion/refresh tests cover extracted text and passages as well as the original file; later modules add their own derived-data tests.

- [ ] Complete the working flow: submit Arabic/English content → inspect extracted text → open a source passage or original PDF page.

**Next:** [Stage 04 — Search and previous checks](04-search-and-fact-checks.md)
