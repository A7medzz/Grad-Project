# Stage 08 — Bilingual Investigation Chatbot

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Select the generation model/provider and its private-data processing settings.
- [ ] Define context/token limits, conversation retention, and JSON or streamed delivery.
- [ ] Define which selected documents/records chat can use and when enabled search is offered.

## Build

### 1. Define scope and conversation state

- [ ] Let the user see/select the current investigation and document scope.
- [ ] Preserve follow-up context, question targets, language preference, and scope changes.
- [ ] Store chat messages under the same access and retention rules as their investigation.
- [ ] Keep user-authored notes and reviewer conclusions distinct from chat/model content.

### 2. Reuse retrieval and analysis services

- [ ] Retrieve only authorized passages and saved analyses relevant to the question.
- [ ] Use stage 04 search and existing investigation services rather than duplicate pipelines.
- [ ] Include provenance, dates, input versions, and analysis/review status in answer context.
- [ ] Answer questions about comparisons/reactions only when those records exist and are accessible.
- [ ] When additional search is needed, state that need and use only enabled, bounded search capabilities.
- [ ] Report unavailable connectors and missing evidence instead of inventing results.

### 3. Generate supported responses

- [ ] Cite stable passage/page references and distinguish source content from AI interpretation.
- [ ] Distinguish historical verdicts, provisional analyses, and reviewer conclusions.
- [ ] State uncertainty, conflicting sources, and stale analysis.
- [ ] Explain reasons summaries as findings about the collected sample only.
- [ ] Support Arabic/English answers while preserving original-language evidence.
- [ ] Check citation validity programmatically and evaluate whether passages actually support answer claims.

### 4. Constrain model actions

- [ ] Treat article/PDF/post instructions as untrusted data, not operational commands.
- [ ] Allow only necessary read/search operations through validated server-side interfaces.
- [ ] Recheck permissions on each tool/retrieval call; never rely on the model to enforce access.
- [ ] Do not silently edit claims, notes, reviewer decisions, or target definitions.
- [ ] Set context, token, time, and resource limits; redact private content from diagnostic logs.

### 5. Build the chat experience

- [ ] Add chat panel, scope indicator, source links, job/stream status, and retry handling.
- [ ] Preserve mixed Arabic/English text, URLs, numerals, and RTL message readability.
- [ ] Make unsupported, partial, and interrupted answers clearly identifiable.
- [ ] Handle context-window truncation without silently substituting a different investigation.

## Data, API, and Screen Checklist

- [ ] Store conversation scope, messages, retrieval references, enabled operations, and model run/configuration.
- [ ] Implement conversation creation, for example `POST /investigations/{investigation_id}/conversations`.
- [ ] Implement authorized message submission/answers, for example `POST /conversations/{conversation_id}/messages`.
- [ ] Implement authorized history, for example `GET /conversations/{conversation_id}/messages`.
- [ ] Connect the chat panel, selected-material control, citation viewer, and search/coverage indicators.

## Check

- [ ] Ask about a PDF claim, prior check, contradicting passage, missing evidence, comparison, and reaction reason.
- [ ] Test follow-ups and scope changes in both languages.
- [ ] Missing supporting material produces an honest limitation rather than a fabricated citation.
- [ ] Malicious document instructions cannot expose another investigation, invoke unauthorized actions, or change records.
- [ ] Citation destinations and answers remain scoped and correctly versioned.
- [ ] Reruns, interrupted streams, source deletion, and stale records have understandable behavior.
- [ ] Measure citation support, faithfulness, appropriate abstention, context handling, latency, and cost.

- [ ] Complete the working flow: select investigation material → ask in Arabic/English → inspect citations → ask a follow-up → handle a question with missing evidence.

**Next:** [Stage 09 — Reporting and public experience](09-reporting-and-public-experience.md)
