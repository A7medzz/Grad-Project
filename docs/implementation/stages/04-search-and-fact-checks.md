# Stage 04 — Bilingual Search and Previous Fact-Check Retrieval

[Master checklist](../README.md)

## Choose for This Feature

- [ ] Select a multilingual embedding model and check its license and execution requirements.
- [ ] Set passage size/overlap, retrieval cutoffs, keyword/semantic fusion, and optional reranking.
- [ ] Select permitted news, primary-document, and previous-check collections/connectors.
- [ ] Define result metadata, contextual-match warnings, and quality/latency measurements.

## Build

### 1. Select and test retrieval methods

- [ ] Build a development collection with relevance judgments in all four query/document language directions.
- [ ] Include claims with no prior check and near-matches with different dates, quantities, entities, or negation.
- [ ] Implement a keyword baseline and compare candidate multilingual embeddings under the same conditions.
- [ ] Inspect Arabic tokenization/normalization rather than assuming default full-text search is adequate.
- [ ] Save corpus/model/chunking settings with evaluation runs so retrieval changes can be compared.

### 2. Implement indexing and query execution

- [ ] Add embedding/index jobs keyed by passage version and model revision.
- [ ] Implement candidate retrieval and a documented keyword/semantic fusion approach.
- [ ] Add reranking only if measured improvement justifies its cost.
- [ ] Apply ownership/visibility constraints before returning candidates or constructing model context.
- [ ] Add source, date, and language filters; define behavior when dates are unknown.
- [ ] Preserve the query target when generating optional query variants.

### 3. Integrate enabled source discovery

- [ ] Combine indexed collections and permitted live discovery under a common result contract.
- [ ] Pass fetched live content through stage 03 before treating its passages as cited evidence.
- [ ] Report connector failures, unavailable content, and coverage separately from empty relevant results.
- [ ] Let disabled or failing social connectors return partial results without breaking core retrieval.

### 4. Implement previous-check matching

- [ ] Import/connect only the selected, permitted historical-check resources.
- [ ] Store each check's original claim, publisher, date, original assessment, URL, and available context.
- [ ] Rank semantic matches and show entity/time/quantity/negation mismatches.
- [ ] Keep any internal label normalization separate from the original publisher's wording.
- [ ] Offer unmatched outcomes and further investigation instead of copying a historical verdict.

### 5. Build search screens

- [ ] Add query/filter controls, ranked excerpts, original-language labels, and citation navigation.
- [ ] Show previous checks in a distinct view with contextual differences.
- [ ] Allow selection of material for investigation or coverage comparison.
- [ ] Explain empty, partial, and unavailable results without claiming that no evidence exists anywhere.

## Data and API Checklist

- [ ] Store passage index metadata, search-run configuration, result references, and historical-check records.
- [ ] Implement scoped search with filters and ranked cited results, for example `POST /search`.
- [ ] Implement historical-check search with context differences, for example `POST /fact-checks/search`.

## Check

- [ ] Evaluate Arabic→Arabic, English→English, Arabic→English, and English→Arabic separately.
- [ ] Report ranking metrics such as nDCG/MRR, recall at selected cutoffs, latency, and cost where applicable.
- [ ] Test near-matches, no-match cases, stale versions, unknown dates, duplicates, and disabled connectors.
- [ ] Confirm unauthorized material never reaches candidates, rerankers, generation context, or result counts.
- [ ] Every displayed excerpt resolves to its source/version and, where applicable, PDF page.
- [ ] A related previous check is not presented as an automatically applicable verdict.
- [ ] Compare keyword, semantic, and combined retrieval on the same cases; record ranking quality and failure examples.

- [ ] Complete the working flow: search across languages → inspect cited results → compare a valid historical check and a misleading near-match → handle no match.

**Next:** [Stage 05 — Claim investigation](05-claim-investigation.md)
