# Stage 06 — Same-Event Coverage Comparison

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Define article-count limits and how the user specifies the event/time period.
- [ ] Select the same-event matching/alignment method and comparison model.
- [ ] Define comparison categories and whether translated excerpts are displayed.

## Build

### 1. Select articles and check the event

- [ ] Accept two or more selected articles and an explicit event/context.
- [ ] Check entities, event time, and document dates before aligning coverage.
- [ ] Distinguish different events, successive developments, and incompatible time windows.
- [ ] Let the user review uncertain event matching rather than forcing a comparison.
- [ ] Identify inaccessible or incomplete article text and explain its impact.

### 2. Align factual content

- [ ] Extract comparable entities, dates, quantities, factual claims, and attributed statements.
- [ ] Align corresponding details across languages while preserving original excerpts.
- [ ] Distinguish agreement, apparent contradiction, and details mentioned by only some sources.
- [ ] Inspect whether an apparent disagreement is a changed time/context rather than a genuine conflict.
- [ ] Keep translated display text separate from original source evidence.

### 3. Generate supported comparisons

- [ ] Link every observation to supporting passage IDs from the relevant article versions.
- [ ] Summarize shared reporting, differing claims, emphasis, and included/omitted details.
- [ ] Describe omission only within the selected available texts, not the publisher's entire coverage.
- [ ] Avoid treating different emphasis as proof of bias, persuasion, or factual error.
- [ ] State insufficient material and uncertainty explicitly.

### 4. Save and present results

- [ ] Persist selected event, input versions, aligned facts, citations, model configuration, and limitations.
- [ ] Build side-by-side article/excerpt views and comparison categories.
- [ ] Allow notes and review without silently changing the original AI output.
- [ ] Mark comparison outputs stale when selected article versions or event scope change.
- [ ] Expose the saved comparison to later chat and report modules through authorized shared services.

## Data, API, and Screen Checklist

- [ ] Store comparison runs, event targets, selected document versions, aligned details, cited observations, and review state.
- [ ] Implement comparison submission, for example `POST /investigations/{investigation_id}/comparisons`.
- [ ] Implement authorized result retrieval, for example `GET /comparisons/{comparison_id}`.
- [ ] Connect the article selector, event confirmation, side-by-side excerpts, and comparison/review panel.

## Check

- [ ] Test same-language and Arabic/English article pairs.
- [ ] Include articles with changed numbers, different event dates, quotations, and successive updates.
- [ ] Different events are flagged; insufficient text is not filled with invented content.
- [ ] Every substantive observation has valid supporting excerpts and preserves attribution.
- [ ] Absence of a detail is not incorrectly described as a denial.
- [ ] Source restrictions and private-investigation access apply to saved comparisons and excerpts.
- [ ] Check same-event detection and whether each generated observation is supported by the cited excerpts.

- [ ] Complete the working flow: select Arabic/English articles → confirm the event → inspect cited agreements/differences → save the comparison.

**Next:** [Stage 07 — Audience-reaction analysis](07-audience-reactions.md)
