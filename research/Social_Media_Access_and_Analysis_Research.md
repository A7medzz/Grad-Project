# Social-media search, sentiment, and stance: feasibility and recommendation

Research date: 21 November 2026. Project: Bilingual Computational Journalism Platform.

Status: researched recommendation, not an approved platform commitment or implemented integration. Prioritize free access; consider a small capped paid pilot. The supervisor requested automatic social-platform search and sentiment/stance analysis. User-submitted posts are an optional additional input.

This review uses current official platform documentation and primary research. It did not test authenticated API calls, measure platform coverage, obtain approvals, create accounts, download datasets, or spend money. Access and pricing must be checked again when implementing. API access, permission to process content, and permission to train models are separate questions.

## Recommended decision

Start with **Bluesky as the first free automatic-search integration**, subject to a short access and Arabic-coverage pilot. Add **X Community Notes downloads as a separate free collection of existing community checks**. Evaluate **Threads public keyword search** if the required permission is approved. Reserve **$10–$20 for an optional X search pilot** if the free sources cannot provide enough useful Arabic material.

This is an engineering recommendation based on access constraints. It is not a measured finding that Bluesky has the best Arabic journalism coverage. Do not make a final Arabic-source decision from API convenience alone.

Retain normal fact-checking articles and primary documents as evidence sources. Social posts can supply claims, reactions, first-party statements, or links to evidence; none becomes reliable merely because it was retrieved.

## Platform comparison

| Source | Automatic discovery | Access and cost evidence | Recommendation for this project |
|---|---|---|---|
| Bluesky | Official API lists post search, threads, quotes, and author feeds. | Public AppView reads generally do not need authentication; public endpoints are available without a published per-post payment schedule in the reviewed API reference. Rate limits still apply. | First free prototype candidate. Confirm search routing/authentication and Arabic usefulness in a pilot. |
| Threads | Public keyword/topic-tag search after approval. | Requires an access token, `threads_basic`, and approved `threads_keyword_search`. Reviewed search documentation lists query limits, not a per-query price. | Strong candidate to test after approval; do not make approval a dependency of the first demo. |
| X | Keyword, hashtag, author, language, URL, and conversation-oriented search. Recent and archive search are documented. | Approved developer account and credentials; pay per use. | Optional capped comparison source. No assumption of free ordinary post search. |
| YouTube | Search videos, then retrieve their available comments and replies. | API quota exists, but additional analysis such as sentiment requires the relevant approved analytics use case. | Later option after analytics approval; video discovery is different from searchable post text. |
| Facebook / Instagram | Meta Content Library provides research search; API access operates in a secure research environment. | Research eligibility/application; API data cannot simply be downloaded into our external app. | Possible separate university research route; poor first backend for this platform. |
| Reddit | External Data API access can support integrations after approval. | External access has a separate application/approval process; Devvit access does not provide external credentials. | Secondary candidate only after approval and a relevance pilot. |
| Telegram | Official API documents public-channel post search. | Current API terms broadly restrict use of Telegram data for AI development and deployment. | Exclude from the proposed AI ingestion pipeline unless appropriate permission resolves that restriction. |
| Mastodon | Search depends on the queried server and its configuration. | Full-text status search needs authentication and an Elasticsearch-backed server; public indexing is opt-in. | Lower priority because search is server-scoped and incomplete across the federation. |

Sources: [Bluesky API reference](https://docs.bsky.app/docs/api/app-bsky-feed-get-feed), [Bluesky rate limits](https://bsky.network/docs/rate-limits/), [Threads search](https://developers.facebook.com/documentation/threads/keyword-search), [X search](https://docs.x.com/x-api/posts/search/introduction), [X pricing](https://docs.x.com/x-api/getting-started/pricing), [YouTube search](https://developers.google.com/youtube/data/reference/rest/v3/search/list), [YouTube analysis permissions](https://developers.google.com/youtube/terms/developer-policies-guide), [Meta API overview](https://developers.facebook.com/docs/content-library-and-api/content-library-api/overview/?locale=de_DE), [Reddit access FAQ](https://developers.reddit.com/docs/guides/faq), [Telegram search](https://core.telegram.org/method/channels.searchPosts), [Telegram API terms](https://core.telegram.org/api/terms), [Mastodon search](https://docs.joinmastodon.org/methods/search/), [Mastodon indexing](https://docs.joinmastodon.org/admin/elasticsearch/).

### Bluesky: first free integration

The official reference lists `app.bsky.feed.searchPosts`, `getPostThread`, and `getQuotes`. Most public GET endpoints can use the public AppView; authenticated requests follow the user's PDS routing. Confirm the exact search endpoint behavior before implementation. Bluesky recommends its cached public API for public-web use cases and documents rate limiting. [API reference](https://docs.bsky.app/docs/api/app-bsky-feed-get-feed), [rate limits](https://bsky.network/docs/rate-limits/).

Use it to prove automatic query generation, post collection, context retrieval, sentiment, and claim-relative stance. Measure Arabic and English relevance separately. An accessible endpoint does not demonstrate sufficient Arabic volume or representativeness.

### Threads: conditional public search

The official endpoint permits public search after permission approval; before approval, search covers only the authenticated user's own posts. It supports top/recent ordering, dates, author and media filters, up to 100 returned items, and 2,200 queries per user per rolling 24 hours across apps. It returns an empty array for keywords deemed sensitive or offensive. Approval, processing permissions, and useful Arabic results remain untested. [Official search documentation](https://developers.facebook.com/documentation/threads/keyword-search).

Replies and conversations have documented endpoints, but their availability for arbitrary discovered posts under our particular token/permissions must be tested. Do not equate keyword-search permission with complete conversation access. [Replies documentation](https://developers.facebook.com/documentation/threads/retrieve-and-manage-replies/replies-and-conversations).

### X: a controlled paid comparison

Recent search covers seven days; full-archive search is documented for pay-per-use and Enterprise access. Preserve the search time range when interpreting missing replies or old claims. [Search documentation](https://docs.x.com/x-api/posts/search/introduction).

The published post-read price is **$0.005 per returned post resource**. The following figures are our arithmetic, not package offers:

| Pilot cap | Theoretical post reads at the published rate |
|---|---:|
| $5 | 1,000 |
| $10 | 2,000 |
| $20 | 4,000 |

These exclude other billable resources, any applicable taxes, and analysis/hosting costs. Reply collection consumes post reads too. Resources may be charged again outside the documented daily deduplication window. Use both a console spending limit and a lower application collection cap; leave auto-recharge off. Recheck the console's actual rates before purchasing. [Pricing](https://docs.x.com/x-api/getting-started/pricing).

Declare the intended analysis in the approved use case. Current terms prohibit training or fine-tuning foundation/frontier models with X API data; do not treat a paid read as permission for model training. Display, deletion, and redistribution obligations also affect saved investigations and exports. [Developer agreement](https://docs.x.com/developer-terms/agreement), [developer policy](https://docs.x.com/developer-terms/policy).

### Why YouTube is conditional

The documented default quota currently separates 100 daily `search.list` calls from 10,000 daily units for other endpoints. Search can find relevant videos; comments are then retrieved for known video IDs, with separate reply retrieval where needed. Disabled comments and unavailable replies must be reported. [Quota documentation](https://developers.google.com/youtube/data/getting-started?hl=en), [comment threads](https://developers.google.com/youtube/data/reference/rest/v3/commentThreads/list), [replies](https://developers.google.com/youtube/data/reference/rest/v3/comments/list).

Google's current policy permits additional derived analytics, explicitly including viewer sentiment, through an approved analytics amendment. It is not automatically permitted merely by having an API key. Raw comment text remains subject to refresh/deletion rules. [Permission requirement](https://developers.google.com/youtube/terms/developer-policies-guide), [derived analytics policy](https://developers.google.com/youtube/terms/derived-metrics-policy).

Public-video discovery also does not grant unrestricted transcript downloads: the captions-download endpoint requires authorization and appropriate permissions. A title or description cannot establish what was said throughout a video. [Caption download](https://developers.google.com/youtube/data/reference/rest/v3/captions/download).

### Other important exclusions

Meta's research API uses a secure environment and restricts downloading its content. Approval for research analysis there does not establish permission to populate this app's database. [Meta API overview](https://developers.facebook.com/docs/content-library-and-api/content-library-api/overview/?locale=de_DE).

TikTok's ordinary Research Tools eligibility lists specific regions and institution requirements; an Egypt-based university team does not qualify simply through being academic. The published regional route therefore cannot be assumed available to us. [Research Tools eligibility](https://developers.tiktok.com/products/research-api/).

Do not build around unofficial scrapers or services advertising unlimited access. A scraper's technical ability does not establish stable platform access or processing rights.

## Social checks and social evidence

Use three distinct roles in the investigation:

1. **Claim source:** the post spreading a statement.
2. **Reaction/context:** posts or replies discussing it.
3. **Evidence or previous check:** a linked primary document, a first-party announcement, a fact-checker's investigation, or a clearly attributed community note.

For a ministry post, establish that the account belongs to the ministry and preserve its exact statement. Such a post may establish what the ministry announced; it does not independently prove every assertion inside the announcement. A verified badge, likes, or repeated posting is insufficient evidence of accuracy.

**Community Notes** provides free downloadable notes, ratings, and status histories. Snapshots are released daily on a best-effort basis and have a documented delay. Use note text, referenced sources, original post ID, status, and snapshot date. Notes are community assessments; a helpfulness status is not a gold truth label. The downloads do not replace keyword search across ordinary X posts or provide their full conversations. [Official data documentation](https://communitynotes.x.com/guide/bg/under-the-hood/download-data).

**Google Fact Check Tools** can search published fact-checks with an API key. Its claim-search endpoint supports text, language, publisher, and age filters. It is useful for finding prior checks concerning a social claim, but it is not a general social-post search API or a guaranteed complete index. [Getting started](https://developers.google.com/fact-check/tools/api), [claim search](https://developers.google.com/fact-check/tools/api/reference/rest/v1alpha1/claims/search?hl=en).

## Sentiment and stance: choose the target explicitly

These should be separate outputs from evidence quality and claim truth.

| Output | Question | Proposed initial labels |
|---|---|---|
| Sentiment | What emotional tone does this post express? | Positive, negative, neutral; flag mixed or unclear cases for review |
| Claim-relative stance | Does the post endorse the selected factual claim? | Supports, denies, questions, comments/no clear position |
| Optional topic stance | Does the author favor a specified policy/topic? | Favor, against, no clear position |
| Evidence assessment | What does the evidence justify about the claim? | Existing project assessment protocol; decided separately |

Example: selected claim: "The bridge has collapsed."

- "Unfortunately, it really has collapsed." Negative sentiment; supports the claim.
- "Thankfully, this is false; the bridge is still open." Positive sentiment; denies the claim.
- "Is there an official confirmation?" Questions the claim; truth remains unresolved.

Sentiment cannot substitute for stance, and stance cannot substitute for verification. Relevance filtering comes before stance: a post about a different bridge is not a denial. Preserve parent/quoted-post context so quoting a rumor is not automatically treated as endorsing it. Avoid classifying individual users' identities or long-term beliefs.

RumourEval provides a primary reference for support/deny/query/comment classification in discussion threads. This is a better initial label design for our claim-investigation workflow than treating all opposition as negative sentiment. [Task paper](https://aclanthology.org/S19-2147.pdf).

## Practical model and dataset candidates

| Component | First candidates | Why / limitation |
|---|---|---|
| Arabic–English sentiment baseline | `cardiffnlp/twitter-xlm-roberta-base-sentiment` | Author model card documents sentiment fine-tuning including Arabic and English; validate our dialects and platforms. |
| Arabic sentiment comparison | `CAMeL-Lab/bert-base-arabic-camelbert-da-sentiment` | Ready sentiment checkpoint fine-tuned on Arabic datasets; useful comparison for dialectal text. |
| Arabic topic stance | MAWQIF and its annotation guidelines | Annotates 4,121 tweets with stance, sentiment, and sarcasm, across a limited target set. It is a dataset, not a universal ready classifier. |
| Arabic cross-target stance evaluation | StanceEval 2026 / Mawqif-XT | Recent task tests transfer to related and unseen targets; inspect release access and splits before adoption. |
| Claim/reply stance design | RumourEval; project-specific annotated examples | Closer to rumor replies; do not assume a news-article or topic-stance model transfers directly. |

Sources: [Cardiff model card](https://huggingface.co/cardiffnlp/twitter-xlm-roberta-base-sentiment), [CAMeLBERT sentiment model card](https://huggingface.co/CAMeL-Lab/bert-base-arabic-camelbert-da-sentiment?library=transformers), [MAWQIF paper](https://aclanthology.org/2022.wanlp-1.16/), [author release](https://github.com/NoraAlt/Mawqif-Arabic-Stance), [StanceEval 2026 overview](https://aclanthology.org/2026.arabicnlp-sharedtasks.65/).

Start with local sentiment inference and a simple documented stance baseline. A pretrained encoder such as MARBERT still needs a task-trained head to become a stance classifier. Compare a compact supervised model with a fixed prompted model only where data-processing permissions and available compute permit. External model APIs add cost independently of social-data access. Check each model/dataset release's license and platform restrictions before downloading, training, or redistributing it.

Do not translate all Arabic into English as the only analysis path. Keep original text; evaluate dialect, Arabizi, code-switching, sarcasm, negation, and quotations. Translation can be an experimental baseline.

## Proposed automatic-search workflow

1. Receive an investigation claim or topic and a date window.
2. Generate short Arabic and English queries, including entity/spelling variants; retain the original claim as the investigation target.
3. Search enabled platform APIs automatically. Return partial results with source-specific errors and coverage limits.
4. Normalize post metadata, preserve original text/context, identify language, deduplicate reposts, and rank for relevance.
5. Retrieve permitted replies/quotes within collection limits. Mark omitted or inaccessible context explicitly.
6. Classify sentiment and stance independently; keep uncertain cases reviewable.
7. Find prior fact-checks and evaluate candidate evidence using the existing investigation pipeline.
8. Show original posts, citations, dates, context, and sample counts. Save only what the source's access terms permit.

Store platform/source ID, canonical URL, creation and retrieval times, original text, language, parent/root links, collection query, source role, target claim ID, model/version, and review state. Distinguish submitted text from content fetched and verified at its original URL. Preserve a reason when a source cannot be refreshed or must be removed.

Describe aggregates as **"stance/sentiment among the retrieved relevant posts"**. Never label them "what Egyptians think" or "public opinion". Search ranking, missing replies, repeated content, access limits, and platform populations bias the sample. Show counts and the date window; do not convert classifier confidence or majority support into a truth percentage.

User-submitted post text plus an optional URL can enter the same analysis pipeline later. A URL alone does not ensure we can fetch its content. Do not invent inaccessible comments, treat screenshots as authenticated posts, or use submission as a workaround for restricted processing rights. Screenshot OCR remains a separate enhancement.

## Immediate next milestone

Run a small collection feasibility pilot before committing to a final platform or a full dashboard:

- Choose 10 investigation topics/claims spanning Arabic and English, including different spellings and claims with no expected match.
- Collect a bounded number of results using Bluesky; record access failures, relevance, Arabic volume, available context, duplicate rate, and elapsed time.
- Inspect Community Notes download structure and estimate storage/indexing needs without assuming the full corpus is small.
- Prepare a Threads access application and test public search only if permission is granted. The owner should review and submit the application.
- If the free sources are insufficient, run the same queries through X under a separately authorized $10–$20 hard cap; compare useful Arabic material per dollar.
- Build a small manually annotated development/evaluation collection, for example 200–300 post–target pairs, clearly presented as a pilot rather than a definitive benchmark. Two annotators label independently and resolve disagreements.

Evaluate retrieval relevance separately from classifier quality. Report sentiment and stance macro-F1, per-class results, uncertainty/abstention, annotation agreement, latency, and cost. Split repeated claims, threads, and translations together; hold out topics/events and report Arabic/English separately. A model trained on MAWQIF's few topics must be evaluated on unfamiliar journalism claims. Proposed numeric success thresholds should be agreed after examining this pilot, not presented as achieved results.

The milestone succeeds when we have one genuinely automatic free-source investigation flow, explicit evidence of its Arabic coverage limitations, and a defensible choice about whether paid access adds enough value. It does not require integrating every platform.
