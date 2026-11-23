# What the 15 papers do, how strong their results are, and what we can build from them

Prepared 20 October 2026. Project: an Arabic-English computational journalism platform for media analysis, semantic retrieval, and evidence-based claim assessment.

This review uses the supplied R01-R15 PDFs as the primary evidence. All numerical results below are reported by the papers; no models were rerun. Page references are one-based PDF pages. Judgments about usefulness and proposed experiments are our analysis, not results established by the papers. This is a review of these 15 papers, not a claim about the best systems available today.

---

## How to interpret the results

### Accuracy:

the fraction of correct labels. It can look high when a common class dominates. Compare with the majority-class baseline on the same split.

### Macro-F1:

compute F1 for each class and average equally. This makes performance on rare classes visible. F1 balances precision (how often a predicted class is correct) and recall (how much of that class is found). Micro-F1 pools decisions, so frequent labels can dominate.

### Success@10:

the fraction of queries with at least one correct match among the first ten results. It does not mean the first result is correct or the system has verified the claim. Recall@100 measures coverage within a much longer candidate list, with the precise relevance definition depending on the benchmark.

### nDCG@10:

a ranking measure that rewards relevant documents near the top. A reported 80.2 on a 0-100 scale is 0.802; it is not 80.2% truth-detection accuracy. Pearson correlation measures association between predicted and human similarity scores, not classification accuracy.

### Evidence-conditioned correctness:

credit requires a correct verdict and adequate evidence. This is stricter and more useful for verification than a label alone. A paper can be an excellent dataset contribution even when its baseline system is weak.

### Fair comparison:

compare systems on the same dataset version, split, language directions, candidate pool, metric, and evidence setting. A multilingual average often measures several same-language tasks; it does not automatically establish Arabic-to-English performance. Scores from 2018, 2023, and 2025 are historical results, not a shared leaderboard.

---

## R01 - AraFacts (2021)

### What and why:

Creates a collection of naturally occurring Arabic claims already assessed by professional fact-checking organizations. The motivation is the shortage of realistic Arabic resources; collecting real rumors avoids relying entirely on invented examples.

### Data and how they obtained it:

The supplied PDF reports 6,222 claims, covering material since 2016, crawled from Misbar (2,952), Fatabyyano (1,503), FactuelAFP Arabic (973), Verify-Sy (707), and Maharat-news (87). They parsed fact-checking web pages for claim text, original verdict, topic, date, article content, claim URLs, and evidence URLs. Keyword heuristics identify text/image/video claim types. The images themselves are not packaged as an image dataset.

### How it works:

Two authors independently mapped 27 original verdict labels to False, Partly-False, Sarcasm, and True, and 35 topic labels to eight categories, then resolved disagreements. Some sites lacked explicit verdicts; for Verify-Sy they inferred a false label from editorial policy and manual inspection of a subset. This is a data construction method, not a trained verifier.

### Results and whether they are good:

The main result is the dataset, metadata, and descriptive analysis; there is no model accuracy to rate. It is a valuable starting resource for Arabic claim search. However, false claims dominate, metadata completeness varies, and many claims concern visual content. Of the collected claim-source URLs, 54.5% are Facebook and 25.0% Twitter; these are URL shares, not the percentage of all claims proved false on those platforms.

### What we can take:

Index the original claims and fact-checks for an Arabic previous-check search feature. Preserve original and normalized verdicts, source, date, and media type. Keep a matched historical fact-check distinct from new independent verification.

### How we can improve and test it:

Deduplicate recurring claims, audit label mappings, separate text-verifiable cases from cases requiring images, and annotate Arabic-English matches plus difficult near-matches with changed dates or numbers. Evaluate Success@10, first-match rank, and false-match rate on a claim-grouped test set. Updating the collection is useful engineering; its research value would need a clear benchmark or method contribution.

### Source and access:

R01.pdf, pp. 1-5; especially Table 1 and Sections 3-5. [Paper](https://aclanthology.org/2021.wanlp-1.26/). [Author dataset pointer](https://gitlab.com/bigirqu/AraFacts/). Version discrepancy: the supplied PDF and its source counts total 6,222, whereas the ACL landing-page abstract says 6,121. Use 6,222 for this PDF and count the actual release before experiments.

### Simplified:

AraFacts is a collection of Arabic claims that journalists have already checked. It is data we can use inside our platform. It contains the claim, its verdict, the fact-checking article, and information such as the source and date.

Think of it as a library of previously investigated rumors.

**For our project, it can answer this question:**

> “Has someone already checked the claim the user is asking about?”

**For example, imagine this situation—this is an illustration, not an actual AraFacts record:**

1. **A user enters:** “Country X is giving every citizen $1,000.”
2. Our platform searches the AraFacts collection.
3. It finds a previously checked claim with the same meaning, even if the wording differs.
4. We display the previous assessment, its explanation, source link, and date.

This helps the user find existing work quickly and gives us a useful feature without collecting thousands of fact-checks ourselves.

#### How would we use it practically?

We would take its Arabic claims and articles, put them in our search database, and use an embedding model such as E5 or BGE-M3 to find matches by meaning. AraFacts supplies the content; the embedding model helps search that content.

For our Arabic–English platform, we could also let someone ask in English and search for an Arabic fact-check. Whether this works reliably is something we would evaluate.

#### What could we improve?

The most useful improvement would be finding the same exact claim, while checking differences in dates, people, places, and numbers. “Every citizen gets $1,000” and “Every student gets $100” sound similar but require different checks.

One limitation matters: if AraFacts has no matching claim, it cannot tell us whether the new claim is true or false. We would then need another part of our platform to retrieve fresh evidence. An old fact-check also needs its date and context checked before applying it to a new post.

So, its role in our project is: give us the initial Arabic fact-check library for a “find previous checks” feature.

---

## R02 - AraStance (2021)

### What and why:

Predicts how an Arabic article relates to a claim: agree, disagree, discuss, or unrelated. A relevant article may mention an event without confirming the specific claim, so relevance alone is insufficient.

### Data and how they obtained it:

4,063 claim-article pairs involving 910 claims: 606 false and 304 true. False claims came from three fact-checking sites in the UAE, Egypt, and Saudi Arabia; true claims were Reuters Arabic headlines, assumed trustworthy and matched approximately by topic and date. Claims date from 2012-2018. They removed multimedia-dependent claims and manually removed debunking language from false-claim headlines.

### How it works:

Google searches used several query forms: the full claim, date restrictions, named entities, and shortened verb-phrase queries. They retained up to ten articles per claim. Native Arabic annotators labeled stance; two labeled each pair and a third resolved disagreement, after a pilot round refined the guidelines. Agreement was kappa 0.67. Four pretrained models were fine-tuned on claim-document pairs with a 512-token maximum. Splits contain 2,848 training, 569 development, and 646 test pairs.

### Results and whether they are good:

ARBERT reached 85% test accuracy and 0.78 macro-F1. Per-class F1 was 0.85 agree, 0.78 disagree, 0.55 discuss, and 0.92 unrelated. Predicting unrelated for every test pair would give 358/646 = 55.4% accuracy (our calculation). The improvement is substantial, but discuss remains weak: the overall score hides difficulty distinguishing context from actual support.

### What we can take:

Use the annotation definitions and data for an Arabic claim-evidence stance component. Its own comparison favors ARBERT; it does not show that the similarly named AraBERT is the winner. Agreement with an article is not proof that the claim is true.

### How we can improve and test it:

Select relevant paragraphs before classification instead of truncating long articles to their opening. Test class weighting or carefully validated additional discuss examples. Hold out claim families and sources, and add bilingual claim-evidence pairs. Measure macro-F1 and discuss/disagree recall with gold passages and retrieved passages separately. Reuters-only true claims create a possible source shortcut, so include source-held-out evaluation.

### Source and access:

R02.pdf, pp. 2-7, Tables 3-6. [Paper](https://aclanthology.org/2021.nlp4if-1.9/). [Dataset](https://github.com/Tariq60/arastance).

### Simplified:

Paper 2, AraStance, helps us understand what an Arabic article says about a particular claim. It provides examples we can use to train and test that ability.

**Suppose the user enters this hypothetical claim:**

> “The university cancelled tomorrow’s exams.”

**Our platform retrieves several articles. AraStance teaches us to distinguish these relationships:**

| What the article says | Relationship to the claim |
| --- | --- |
| “The university announced that tomorrow’s exams are cancelled.” | Agree |
| “The university confirmed that exams will take place tomorrow.” | Disagree |
| “Students have requested cancellation, but no decision has been announced.” | Discuss |
| “The university opened a new library.” | Unrelated |

This matters because an article mentioning exams is not necessarily evidence that exams were cancelled.

The authors collected 4,063 claim–article pairs, had Arabic speakers label those relationships, and fine-tuned language models to predict them. Their best model achieved 85% accuracy and 78% macro-F1. It struggled most with the Discuss category.

#### How we could use it:

1. Our search component retrieves articles about the user’s claim.
2. A model trained using AraStance reads each claim–article pair.
3. Our platform shows which articles agree, disagree, merely discuss the issue, or are unrelated.
4. We display the relevant passages so users can inspect them.

How we could improve it: identify the relevant paragraphs in long articles, improve the difficult Discuss category, and add Arabic–English examples. AraStance itself does not establish performance when the claim and article are in different languages.

One distinction is essential: an article agreeing with a claim does not prove the claim is true. The article could itself be wrong. We still need to assess the evidence, source, date, and context.

**So the two papers serve different parts of our platform:**

- **AraFacts:** “Has this claim already been checked?”
- **AraStance:** “How does this article relate to this claim?”

---

## R03 - AVeriTeC (2023)

### What and why:

Verifies real-world claims using web evidence, with intermediate questions, sourced answers, and a justification. It addresses three problems in earlier data: claims needing missing context, inadequate evidence, and evidence published after the claim.

### Data and how they obtained it:

4,568 English claims from fact-checks by 50 organizations. Starting with 8,000 fact-check articles, they removed duplicates and inaccessible articles, normalized claims, and used a multi-stage human annotation process. One annotator assembled question-answer evidence; another judged whether it was enough for a verdict. Mismatches triggered reannotation and sometimes removal. Evidence URLs were archived. The chronological splits are 3,068 train, 500 development, and 1,000 test claims.

### How it works:

The baseline generates search questions, searches Google with date constraints, ranks sentences with BM25, generates question-answer candidates, reranks them with BERT, predicts evidence stance, combines this into one of four verdicts, and generates a BART explanation. Verdicts are supported, refuted, conflicting evidence/cherry-picking, and not enough evidence.

### Results and whether they are good:

In the supplied November 2023 v3 PDF, Table 3 reports evidence-conditioned verdict accuracy of 0.15 at evidence threshold 0.25, versus 0.49 using gold evidence. Table 4 separately reports label macro-F1 of 0.23 for the baseline and 0.49 with gold evidence. The identical 0.49 values belong to different metrics. The end-to-end baseline is weak, while the dataset and diagnostic comparison are valuable. In a manual sample of 20 development claims, normal search found useful evidence for 9; using gold questions found it for 16. This small sample suggests query quality matters, without establishing a population-wide improvement rate.

### What we can take:

Decompose a claim into answerable questions, retain source-backed answers, and expose how the evidence supports the verdict. Compare gold-evidence and retrieved-evidence performance to locate the bottleneck.

### How we can improve and test it:

Compare claim-only queries with generated subquestions, then compare lexical, dense, hybrid, and reranked retrieval. Test Arabic plus English evidence against same-language evidence. Check whether generated answers are actually entailed by their cited passages: the paper explicitly says its metric does not ensure this. Measure evidence-conditioned correctness and human-judged citation support, with a frozen, dated evidence corpus.

### Source and access:

R03.pdf, pp. 3-10, especially Tables 2-4. [Paper](https://arxiv.org/abs/2305.13117). [Data and baseline](https://github.com/MichSchli/AVeriTeC). These are the supplied paper's baseline scores, not later shared-task results.

### Simplified:

Paper 3, AVeriTeC, shows how to investigate a claim using evidence from the web—even when we cannot find an existing fact-check. It provides both a dataset and a baseline system for this process.

**Imagine the user enters this hypothetical claim:**

> “The university cancelled all exams tomorrow.”

Searching for similar articles is only the beginning. We need to establish which university, which exams, and which date.

**AVeriTeC breaks the investigation into smaller questions:**

| Question | Evidence we would look for |
| --- | --- |
| Did the university announce a cancellation? | An announcement from the university |
| Does it cover all exams or only certain departments? | The announcement’s exact wording |
| Does it apply to tomorrow’s date? | The announcement’s date and effective period |

**The system then combines the answers into an assessment and explains it using the evidence. For example:**

> “The announcement cancels exams in one department only. It does not support the claim that all exams were cancelled.”

#### What data did the authors create?

They collected 4,568 real-world English claims from fact-checks by 50 organizations. Human annotators wrote investigation questions, found source-backed answers, and provided verdicts and explanations. Other annotators checked whether the evidence was sufficient.

#### How can we use it in our project?

**We can adopt this workflow:**

1. Break the user’s claim into specific questions.
2. Search Arabic and English sources for answers.
3. Keep the relevant passages, URLs, and dates.
4. Assess whether the evidence supports, contradicts, conflicts, or remains insufficient.
5. Show a short explanation linked to those passages.

The dataset can help us develop and evaluate the English part. Its original results do not establish Arabic performance.

#### Were its results good?

The original baseline scored 0.15—15%—when a correct verdict also required adequate retrieved evidence. With human-provided evidence, it reached 49% under that evaluation. That is weak end-to-end performance, but it reveals an important problem: finding the right evidence is difficult.

Our improvement opportunity: test whether searching both Arabic and English retrieves better evidence than searching one language alone, then measure whether that actually improves the final assessments.

#### The connection between the three papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need to investigate it, and what conclusion does that evidence justify?

---

## R04 - FEVER (2018)

### What and why:

Establishes large-scale fact verification in which the system must both judge a claim and supply supporting or refuting sentences. It makes evidence retrieval part of the task.

### Data and how they obtained it:

185,445 English claims created by human annotators using the June 2017 Wikipedia dump. Annotators wrote claims from introductory sentences and mutated them through paraphrase, negation, entity/relation replacement, and specificity changes. Other annotators assigned Supported, Refuted, or Not Enough Information and selected evidence. Fifty annotators participated across the stages; five-way label agreement was kappa 0.6841. Source pages used to generate claims were kept in only one split.

### How it works:

A three-stage baseline retrieves five Wikipedia documents using TF-IDF/DrQA, selects five sentences, and runs a decomposable-attention textual entailment model. To train the insufficient-information class, they paired such claims with retrieved nearby sentences or random sentences. Nearby, topically related negatives made the full pipeline more robust than easy random negatives.

### Results and whether they are good:

On the original test set, the pipeline achieved 50.91% label-only accuracy and 31.87% when correct evidence was also required. Complete-evidence recall was 45.89%, precision 10.79%, and F1 17.47%. This was an important benchmark contribution, but a weak complete verification system. The 19.04-percentage-point gap (our subtraction) shows why reporting just correct labels overstates usable performance. Do not compare the evidence-aware score directly with random label guessing, because the evidence requirements differ.

### What we can take:

Use explicit insufficient-evidence labels, evidence-aware scoring, and difficult negative examples. Preserve alternative valid evidence sets where possible; exact-match evidence annotations can miss legitimate answers.

### How we can improve and test it:

Replace the old retrieval baseline with controlled dense/hybrid comparisons and test multi-sentence evidence selection. Keep synthetic Wikipedia evaluation separate from naturally occurring Arabic-English news. Include near-matches sharing entities but changing time, quantity, or relations. Compare complete-evidence recall and evidence-conditioned verdict accuracy, not only classification accuracy.

### Source and access:

R04.pdf, pp. 3-8; especially Sections 3, 5.2, and 5.7. [Paper and resource pointers](https://aclanthology.org/N18-1074/).

### Simplified:

Paper 4, FEVER, teaches a system to check a claim against Wikipedia and show the sentences that justify its decision. Its central idea is that a verdict should come with evidence.

**For example:**

> **Claim:** “The Eiffel Tower is in Rome.”

> **Retrieved evidence:** A Wikipedia sentence placing it in Paris.

> **Decision:** Refuted.

**FEVER uses three labels:**

| Label | Meaning |
| --- | --- |
| Supported | The evidence supports the claim. |
| Refuted | The evidence contradicts the claim. |
| Not Enough Information | The available evidence cannot establish either conclusion. |

Not finding evidence does not mean a claim is false.

#### Where did its data come from?

The authors created 185,445 English claims. Human annotators read Wikipedia sentences and wrote claims based on them. They also changed details—such as an entity or relationship—to create different examples. Annotators then assigned labels and selected evidence sentences.

Unlike AraFacts, these are largely constructed examples, rather than rumors collected from real-world fact-checking.

#### How does its system work?

1. Find Wikipedia pages related to the claim.
2. Select the relevant sentences.
3. Compare those sentences with the claim.
4. Return a label and evidence.

#### Were the results good?

**The original system achieved:**

- 50.91% accuracy when only the verdict mattered.
- 31.87% when the verdict also needed correct evidence.

That is weak complete-system performance. However, FEVER’s major contribution is the dataset and evaluation method: it makes it harder for a system to receive credit for an unsupported answer.

#### How does it relate to us?

**We can use it as a supporting English training/testing resource and adopt its evaluation principle:**

> “Did our platform reach the right conclusion and provide evidence that actually justifies it?”

We should also compare performance with human-selected evidence against automatically retrieved evidence. This tells us whether our main problem is finding evidence or understanding it.

#### How could we improve on its setting?

Test naturally occurring Arabic–English news claims, retrieve evidence beyond Wikipedia, and handle dates, quantities, and multiple sources.

The difference from paper 3 is simple: FEVER provides a controlled Wikipedia-based task; AVeriTeC investigates real-world claims using web evidence and intermediate questions.

---

## R05 - SemEval-2025 Task 7: fact-checked claim retrieval

### What and why:

Finds an existing fact-checked claim matching a new social-media post, within or across languages. This reduces repeated fact-checking work. It does not independently verify a completely new claim.

### Data and how they obtained it:

Built from MultiClaim: 205,751 fact-checked claims from 142 organizations and 28,092 posts. They linked posts to checks through ClaimReview metadata or Facebook/Instagram fact-check warnings, rather than treating arbitrary semantic similarity as a gold match. OCR and English translations were produced with Google services. New test data added more organizations and Telegram. The final test has 4,276 monolingual posts and 4,000 crosslingual-track posts; candidate pools differ between tracks.

### How it works:

This is a shared-task overview, not one algorithm. Teams used multilingual embeddings or English translation, contrastive fine-tuning with hard negatives, and sometimes reranking or model ensembles. The crosslingual track also contains same-language pairs, so its score is not exclusively different-language performance. Success@10 counts a query as successful if at least one annotated match appears in the first ten results.

### Results and whether they are good:

The winning submitted scores were 0.9601 mean monolingual Success@10 and 0.85875 in the crosslingual track. E5-Large baselines were 0.8589 and 0.63725 respectively, so the crosslingual gain was 22.15 percentage points (our calculation). In monolingual Arabic, the winner scored 0.986 versus E5's 0.916. These are strong retrieval results; they do not establish first-result reliability or truth accuracy. The Arabic-English portions are small: Table 4 lists 39 Arabic-post/English-claim pairs and 14 in the reverse direction in the crosslingual test.

### What we can take:

This is the closest reference for our bilingual previous-fact-check search. Reuse its task formulation and compare direct multilingual search against translation-based search.

### How we can improve and test it:

Add enough genuine Arabic-English examples to report both directions reliably, plus queries with no existing match. Evaluate Success@1/10, MRR, false-match rate, latency, and results by language direction. Mine difficult negatives differing only in numbers, people, or dates. Check missing relevance labels manually before calling an unannotated match wrong. Keep original-post, OCR, and translation inputs as separate ablations.

### Source and access:

R05.pdf, pp. 2-8, Tables 2-4 and 8-9. [Paper](https://aclanthology.org/2025.semeval-1.323/). [Official dataset pointer](https://doi.org/10.5281/zenodo.14989176). The paper describes research access; verify the release's access conditions before depending on it.

### Simplified:

Paper 5 is about finding an existing fact-check—even when the user’s claim and the fact-check are in different languages. This makes it directly relevant to our Arabic–English platform.

**Imagine this hypothetical example:**

> **User’s Arabic post:** “الدولة س ستمنح كل مواطن ألف دولار”

> **Existing English fact-check:** “Country X will give every citizen $1,000.”

The wording and language differ, but they describe the same claim. The system should find that English fact-check for the Arabic user.

**The paper studies two situations:**

| Situation | Example |
| --- | --- |
| Same-language retrieval | Arabic post → Arabic fact-check |
| Cross-language retrieval | Arabic post → English fact-check, or the reverse |

#### Where did the data come from?

The task builds on MultiClaim, containing approximately 206,000 fact-checked claims and 28,000 social-media posts.

The authors connected posts to relevant fact-checks using links recorded by fact-checkers and platform fact-checking warnings. They also collected additional examples for testing. These connections tell the system which fact-check it should retrieve for each post.

#### How do the systems work?

**This paper describes a competition involving multiple approaches. Common approaches were:**

- Convert posts and fact-checked claims into meaning-based vectors, then search for close matches.
- Translate both texts into English before searching.
- Train models using matching pairs and difficult non-matching examples.
- Rerank the initial search results more carefully.

#### Were the results good?

**The best submitted systems achieved approximately:**

- 96% Success@10 in the same-language track.
- 85.9% Success@10 in the crosslingual track.

Success@10 means at least one correct fact-check appeared among the first ten results. It does not mean the first result was correct or that the system verified the claim’s truth. These are strong results across the benchmark; they are not specifically Arabic–English scores.

#### How can we use it?

It gives us a direct design and evaluation reference for our “Find previous fact-checks” feature. We could compare E5 and BGE-M3 search against a translation-based approach.

#### What could we improve?

Test Arabic → English and English → Arabic separately, handle claims with no existing match, and distinguish near-matches with different dates, people, or quantities.

The connection to paper 1 is straightforward: AraFacts supplies an Arabic fact-check collection; paper 5 shows how to search collections like it across languages.

---

## R06 - MIRACL (2023)

### What and why:

Builds a human-judged search benchmark across 18 languages, including Arabic and English, to improve information retrieval beyond English. Queries and relevant passages are in the same language.

### Data and how they obtained it:

Approximately 78,000 queries and 726,000 relevance judgments over Wikipedia. It extends Mr. TyDi with richer passage judgments and seven additional languages. Native speakers wrote natural questions from short article prompts before seeing retrieval results, then assessed candidate passages. Candidates were pooled from BM25, mDPR, and mColBERT. Most pairs received one annotation, with automatic checks and separate review of samples.

### How it works:

Compares lexical retrieval, dense retrieval, multi-vector retrieval, and a hybrid combining normalized BM25 and mDPR scores. The resource supplies train/development/test splits and judged negatives as well as positives. Queries with no relevant passage found in the judged candidates were excluded, which means the main benchmark does not test rejection of unanswerable queries.

### Results and whether they are good:

On the Arabic development set, BM25 achieved nDCG@10 of 0.481, mDPR 0.499, and their hybrid 0.673. Arabic Recall@100 rose to 0.941 with the hybrid, compared with 0.889 for BM25 and 0.841 for mDPR. Across all 18 languages, hybrid nDCG@10 was 0.566 versus 0.385 for BM25. This is a clear, useful result on that benchmark: combining methods improves ranking substantially. It does not establish Arabic-to-English retrieval or factual correctness.

### What we can take:

Adopt the pooling and human relevance-judgment approach, and include a strong lexical baseline. Treat Arabic and English retrieval as separately reportable tasks, rather than hiding one behind a multilingual average.

### How we can improve and test it:

Build an analogous news-based evaluation with all four directions: Arabic-to-Arabic, English-to-English, Arabic-to-English, and English-to-Arabic. Include unavailable-answer cases instead of discarding them. Measure nDCG@10, evidence recall, and latency under identical corpus and query conditions. Review samples from every retrieval method to reduce bias toward the system that supplied annotation candidates.

### Source and access:

R06.pdf, pp. 4-8, 10, and 12; especially Table 5. [Paper](https://aclanthology.org/2023.tacl-1.63/). [Official repository](https://github.com/project-miracl/miracl).

### Simplified:

Paper 6, MIRACL, helps us measure whether our search system retrieves useful passages. It provides a dataset for training and testing retrieval across 18 languages, including Arabic and English.

**Suppose someone asks:**

> “When was this university established?”

**Our search system might return:**

| Retrieved passage | Useful? |
| --- | --- |
| “The university was established in 1995.” | Relevant: answers the question |
| “The university opened a new library in 2020.” | Not relevant: mentions the university but lacks the answer |

MIRACL teaches us to distinguish text about the same topic from text that actually answers the question.

#### What data did they use, and how did they obtain it?

They collected approximately 78,000 questions and 726,000 question–passage relevance judgments over Wikipedia.

Native speakers wrote questions. Several search methods retrieved candidate passages, and human annotators judged whether those passages were relevant.

One essential distinction: MIRACL evaluates same-language search. An Arabic question searches Arabic passages; an English question searches English passages. It does not directly test Arabic → English retrieval.

#### What methods and results did they compare?

**For Arabic search:**

| Method | How it searches | nDCG@10 |
| --- | --- | --- |
| BM25 | Matches words | 0.481 |
| mDPR | Matches learned meaning representations | 0.499 |
| Hybrid | Combines both | 0.673 |

Here, nDCG@10 measures how well useful passages are placed near the top. 0.673 is a ranking score, not 67.3% fact-checking accuracy.

The hybrid result is substantially better in this comparison. That suggests combining word matching and meaning matching is worth testing in our platform.

#### How does it connect to the previous papers and our project?

| Paper | Role in our platform | Connection to MIRACL |
| --- | --- | --- |
| 1 — AraFacts | Supplies existing Arabic claims and fact-checks | Gives us content to search; MIRACL contributes retrieval evaluation ideas. |
| 2 — AraStance | Determines whether an article agrees, disagrees, or discusses a claim | We must retrieve useful articles before comparing their stance. |
| 3 — AVeriTeC | Investigates claims through questions and sourced answers | MIRACL helps us evaluate the passage-search step. |
| 4 — FEVER | Requires evidence alongside verdicts | Better retrieval can help supply that evidence, although relevance alone cannot establish truth. |
| 5 — Claim retrieval | Finds a previously checked version of a claim | It evaluates matching claims; MIRACL evaluates finding relevant passages. These need separate tests. |

#### What can we improve?

Build a smaller MIRACL-style evaluation using news passages, then test all four directions: Arabic → Arabic, English → English, Arabic → English, and English → Arabic. Include cases where no passage answers the question.

For us, MIRACL’s main contribution is a way to answer: “Is our search finding the evidence we need, and ranking it where users can find it?”

---

## R07 - M3-Embedding / BGE-M3

### What and why:

Creates one embedding model that supports more than 100 languages, multiple text lengths, and three retrieval modes. This reduces the need for a different model for every retrieval representation.

### Data and how they obtained it:

The supplied revised paper describes 1.2 billion weakly supervised text pairs across 194 languages and 2,655 crosslingual correspondences. Sources include Wikipedia, S2ORC, xP3, mC4, CC-News, NLLB, and CCMatrix. Training also uses labeled search/QA/NLI data including MIRACL and Mr. TyDi. GPT-3.5-generated questions paired with long source articles supply additional long-document training examples. These are training resources, distinct from the evaluation benchmarks.

### How it works:

Dense retrieval represents each text with one vector. Sparse retrieval learns token weights for matching terms. Multi-vector retrieval compares contextual token representations. Self-knowledge distillation combines their scores into a training signal. It supports inputs up to 8,192 tokens; the paper uses expensive multi-vector scoring as a reranker over dense candidates.

### Results and whether they are good:

MIRACL Arabic development nDCG@10 is 78.4 for dense retrieval and 80.2 for all three modes, compared with 76.0 for mE5-large in the same table. On MKQA, which actually searches English Wikipedia from non-English queries, Arabic Recall@100 is 71.1 dense and 71.5 for all modes, versus 68.7 for mE5-large. Thus the model has useful same-language and cross-language evidence; the extra modes add only 0.4 points on that Arabic MKQA result. These are strong retrieval results in the reported comparisons, with no proof yet of claim-verification quality.

### What we can take:

Test a pretrained checkpoint as a practical bilingual search candidate. The major value is its multilingual dense representation; extra retrieval modes should earn their computational cost in our application.

### How we can improve and test it:

Start with dense search, then add BM25 or learned sparse signals, then rerank. Compare short overlapping passages against whole articles so supporting details are not diluted. Measure all four language directions, names/numbers/dates, latency, memory, and index size. Do not assume sparse retrieval alone bridges Arabic and English: the paper observes limited term overlap across languages.

### Source and access:

R07.pdf, pp. 3-8, Tables 1-4. The supplied file is arXiv v5 dated 12 December 2025, from a work first posted in 2024. [Paper](https://arxiv.org/abs/2402.03216). [Implementation](https://github.com/FlagOpen/FlagEmbedding). [Model](https://huggingface.co/BAAI/bge-m3).

### Simplified:

Paper 7, BGE-M3, gives us an actual AI model for searching by meaning across languages. It can help an Arabic query retrieve an English passage, even when they share no words.

**For example, imagine:**

> **Arabic query:** “متى تأسست الجامعة؟”

> **English passage:** “The university was founded in 1995.”

A keyword search would struggle because the languages differ. BGE-M3 represents their meaning as numerical vectors, allowing the system to identify a connection. We would still need to identify which university the user means.

#### What does “M3” mean?

**The model supports three capabilities:**

- **Multiple languages:** more than 100 languages.
- **Multiple search methods:** meaning-based search, learned keyword matching, and detailed token-level comparisons.
- **Multiple text lengths:** short sentences through documents of up to 8,192 tokens.

We can start with its simpler meaning-based search and test whether the additional methods improve our results enough to justify their cost.

#### What data did the authors use?

The supplied paper describes approximately 1.2 billion training text pairs, collected from resources including Wikipedia, news, scientific articles, web text, and translation datasets.

Examples of training pairs include a title and its article, or equivalent sentences in different languages. They also used labeled retrieval datasets, including MIRACL, and generated questions for long-document training.

This teaches the model which texts should have similar representations.

#### Were the results good?

**On Arabic MIRACL search, it achieved:**

- 78.4 nDCG@10 using meaning-based retrieval.
- 80.2 when combining all three retrieval methods.

These are ranking scores on a 0–100 scale, not truth-detection accuracy.

The paper also tested genuine Arabic-to-English retrieval on MKQA. Its meaning-based model achieved 71.1 Recall@100. That is useful cross-language evidence, but retrieving a passage somewhere among 100 candidates is easier than putting it first.

#### How can we use it, and improve further?

Use a pretrained BGE-M3 model to index our fact-checks and evidence passages, then search them using Arabic or English queries.

**We could improve the complete search pipeline by testing:**

- Smaller passages instead of whole articles.
- A second model that carefully reranks the initial results.
- Checks for mismatched people, dates, and numbers.

We must measure whether these changes help on our own news data. Similar meaning does not guarantee the same claim—or a true claim.

#### The connection between the seven papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need to investigate it, and what conclusion does that evidence justify?
- **FEVER:** Can we provide evidence that actually supports our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search retrieve and rank relevant passages?
- **BGE-M3:** What model can we use to perform that meaning-based, multilingual search?

---

## R08 - Multilingual E5 (2024)

### What and why:

Produces reusable multilingual embeddings in small, base, large, and instruction-tuned variants, allowing a tradeoff between search quality and inference cost.

### Data and how they obtained it:

Around one billion weakly supervised training pairs from Wikipedia titles/passages, web-page titles/content in mC4, news headlines/articles, NLLB translations, Reddit comments/replies, scientific titles/abstracts and citations, StackExchange question/answer pairs, xP3, and other collections. Supervised fine-tuning uses roughly 1.6 million examples from retrieval, QA, NLI, FEVER, MIRACL, and Mr. TyDi. The instruction-tuned variant additionally uses 500,000 synthetic examples covering 93 languages.

### How it works:

Contrastive learning pulls matching texts together and separates them from negatives. Large batches provide in-batch negatives; supervised training adds mined hard negatives and distillation from a cross-encoder, which jointly reads a query and a candidate document. The standard large model's official usage specifies query/passage roles through input prefixes; the document prefix is `passage:`.

### Results and whether they are good:

On 16 MIRACL development languages, mE5-large achieved mean nDCG@10 of 66.5 and Recall@100 of 94.3, versus BM25 at 39.3 and 78.7. Arabic nDCG@10 was 76.0 and Recall@100 97.3. The instruction variant reached 64.4 mean on the English MTEB benchmark versus 61.5 for standard large, but its MIRACL mean was slightly lower: 65.7 versus 66.5. Strong results, with no universally best variant. A 16-language average must not be directly ranked against an 18-language average elsewhere.

### What we can take:

Use E5 as a serious competing embedding baseline alongside BGE-M3. Smaller variants may be sufficient for an interactive graduation-project demo, but measure this on our hardware.

### How we can improve and test it:

Compare standard large, a smaller model, and the instruction variant on the same held-out news/claim queries. Preserve dates, numbers, negation, and names in preprocessing. Test translation and reranking as controlled additions. For the standard model, use `query:` and `passage:` prefixes even for Arabic; follow the distinct model-card instructions for instruction variants. Record both ranking quality and query latency; do not select a model from one aggregate score.

### Source and access:

R08.pdf, pp. 1-2, 4-5, Tables 1-6. [Paper](https://arxiv.org/abs/2402.05672). [Official implementation](https://github.com/microsoft/unilm/tree/master/e5). [Standard large model usage](https://huggingface.co/intfloat/multilingual-e5-large).

### Simplified:

Paper 8, Multilingual E5, gives us another model for searching by meaning across languages. For our project, it is an alternative to BGE-M3 that we should compare on the same Arabic–English queries.

**Imagine this example:**

> **Arabic query:** “ارتفاع أسعار الوقود”

> **English article:** “Fuel prices have increased.”

E5 converts both texts into numerical representations called embeddings. Texts with related meanings should have similar representations, allowing us to find the article despite the different languages and wording.

#### Why did the authors develop it?

Many embedding models focused mainly on English. The authors wanted multilingual models that could also offer different balances between search quality and computational cost.

They released small, base, and large models, plus an instruction-tuned variant.

#### What data did they use, and how did they train it?

**Training had two main stages:**

1. Learn general relationships from around one billion text pairs. Examples include news headlines paired with articles, questions paired with answers, and translated sentences.
2. Fine-tune using roughly 1.6 million labeled examples. These include retrieval and question-answering datasets, with resources such as MIRACL.

The model learns to bring matching texts closer together and separate them from non-matches. Difficult non-matches help it distinguish genuinely useful passages from passages that merely share a topic.

#### Were the results good?

**On MIRACL:**

- The large model achieved 66.5 average nDCG@10 across 16 languages, compared with 39.3 for BM25.
- Its Arabic nDCG@10 was 76.0.

These are strong ranking results in that evaluation. However, MIRACL measures same-language retrieval, so these particular scores do not establish Arabic-to-English performance.

Also, the instruction-tuned model did not win every comparison: its MIRACL average was slightly below the standard large model. A more elaborate variant is not automatically better for our task.

#### How can we use it?

We can use an existing E5 checkpoint to search our fact-check and evidence collections. We do not need to repeat its billion-pair training.

**Our experiment would compare E5 and BGE-M3 using identical data and queries, measuring:**

- Whether they retrieve the correct claim or evidence.
- How highly they rank it.
- Arabic-to-English and English-to-Arabic performance separately.
- Response time and memory requirements.

Then we could test whether translation, reranking, or training on our claim examples improves the results.

#### The connection between the eight papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need to investigate it, and what conclusion does that evidence justify?
- **FEVER:** Can we provide evidence that actually supports our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search retrieve and rank relevant passages?
- **BGE-M3:** What model can perform meaning-based, multilingual search?
- **Multilingual E5:** What alternative search model should we compare against BGE-M3 for our project?

---

## R09 - SemEval-2022 Task 8: news article similarity

### What and why:

Estimates whether two articles cover the same substantive story across languages. Two articles can mention the same people yet describe different events or different years, so generic text similarity is insufficient.

### Data and how they obtained it:

9,820 annotated article pairs: 4,918 train and 4,902 evaluation, across 18 language combinations. The source pool was roughly 60 million Media Cloud articles in ten languages from January-June 2020. They filtered articles, extracted named entities, linked entities across languages using Wikidata, and sampled plausible pairs using entity overlap and temporal information. Twenty-five annotators rated geography, entities, time, narrative, overall similarity, style, and tone on a four-point scale.

### How it works:

Participating systems predicted overall similarity using pretrained multilingual encoders, fine-tuning, translation, and sometimes entity or other features. Evaluation compared predictions with averaged human overall-similarity judgments using Pearson correlation. Overall similarity explicitly excludes style, framing, and tone, even though those other dimensions were separately annotated.

### Results and whether they are good:

Leading systems reached about 0.818 Pearson correlation. Top annotators' correlations with others' judgments were roughly 0.85-0.88, although those human comparisons use different subsets and should not be treated as an exact model ceiling. Overall annotation agreement was Krippendorff's alpha 0.77. These are strong story-similarity results, not 81.8% event-clustering accuracy. The dataset has 572 Arabic pairs (274 train, 298 evaluation), but no Arabic-English pair subset in the published distribution.

### What we can take:

Use its seven-dimensional guidelines and entity/date-aware sampling to build event grouping. Group coverage of an event first, then compare how outlets frame that event.

### How we can improve and test it:

Annotate Arabic-English pairs, including same-topic/different-event and same-event/different-framing examples. Compare embeddings alone with embeddings plus entity and date features. Evaluate pairwise similarity and, separately, the resulting clusters using pairwise precision/recall or B-cubed F1. Hold out events, and test updates to an evolving event rather than only static pairs.

### Source and access:

R09.pdf, pp. 2-7 and abstract, Tables 3-5. [Paper](https://aclanthology.org/2022.semeval-1.155/). [Dataset/codebook](https://zenodo.org/records/6507872). The paper distributed URLs and a downloader rather than unrestricted raw article text; account for broken pages and access conditions.

### Simplified:

Paper 9 helps us identify articles covering the same news story—even when their wording or language differs. For our platform, this supports grouping coverage of an event so users can compare different outlets.

**Imagine these hypothetical headlines:**

> **Arabic article:** “الجامعة س تفتتح مكتبة جديدة”

> **English article:** “University X opens its new library.”

They probably describe the same event. But an article about another university opening a library, or the same university opening one five years earlier, should belong to a different group.

**The task is therefore more specific than finding articles about the same topic:**

> “Are these articles describing the same actual story?”

#### What data did the authors use?

They created 9,820 annotated article pairs, sampled from roughly 60 million news articles collected through Media Cloud.

**Human annotators compared the pairs across seven dimensions:**

- Location.
- People and organizations.
- Time.
- Narrative.
- Overall story similarity.
- Writing style.
- Tone.

The overall story-similarity judgment explicitly excludes style, framing, and tone. Two outlets can describe the same event very differently.

#### How did the systems work?

This is a shared-task paper describing multiple competing systems. Approaches used multilingual language models, translation, and sometimes additional features such as named entities.

These methods predict a similarity score for each article pair. In our platform, such scores could help form event groups—but grouping many articles would need its own evaluation.

#### Were the results good?

Leading systems achieved approximately 0.818 Pearson correlation with human similarity judgments.

That is strong agreement for this task. It does not mean 81.8% accuracy at grouping articles or checking facts.

The dataset includes Arabic articles, but its published distribution contains no Arabic–English article-pair subset. We would need additional examples to evaluate our particular bilingual use case.

#### How can we use it and improve further?

We could build a feature where users open one event and see related Arabic and English coverage.

**Our experiment would compare:**

1. E5 or BGE-M3 similarity alone.
2. Similarity combined with matching people, places, and dates.

We should test difficult cases: the same people involved in different events, old stories recirculating, and updates to an ongoing event.

Once articles are correctly grouped, other components could compare their framing or persuasion techniques. This paper does not itself establish whether an outlet is biased or an article is true.

#### The connection between the nine papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which Arabic and English articles cover the same event, so we can compare their coverage?

---

## R10 - ArAIEval (2023)

### What and why:

Benchmarks Arabic persuasion detection and a separate set of tweet-content classifications. It supports analysis of how content is written; these tasks must not be collapsed into a single factual-truth label.

### Data and how they obtained it:

Persuasion uses 3,189 snippets: 2,427 train, 259 development, and 503 test. These combine news-source tweets and paragraphs parsed from news pages linked through AraFacts. Three annotators independently marked 23 persuasion techniques, then two consolidators established gold annotations. Binary labels indicate whether any technique is present. The separate tweet dataset has 19,991 examples from COVID-related posts collected in February-March 2020; fine-grained categories are hate speech, offensive content, rumor, and spam.

### How it works:

A shared-task overview of systems mostly fine-tuning Arabic transformers, with variants using class weights, focal/Dice losses, augmentation, or ensembles. One successful binary persuasion system used an auxiliary tweet-versus-news classification task. The binary and multilabel persuasion settings share snippets but have different objectives.

### Results and whether they are good:

Best binary persuasion micro-F1 was 0.7634 versus majority 0.6581. Best multilabel micro-F1 was 0.5666 versus majority 0.3599, but that winning system's macro-F1 was only 0.2156; another system reached macro-F1 0.2497. Thus common techniques are learned better than rare ones. The separate binary tweet task reached 0.9048 micro-F1, and its four-category task reached 0.8356. Those high tweet scores do not mean 90% accuracy at evidence-based fact-checking or demonstrated detection of deceptive intent.

### What we can take:

Use the Arabic persuasion dataset and technique taxonomy for a separate media-analysis panel. It is directly useful for labels such as loaded language and name calling, with appropriate uncertainty for rare techniques.

### How we can improve and test it:

Add carefully adjudicated examples for rare labels, tune class-specific thresholds on development data, and evaluate macro-F1 and per-label precision/recall alongside micro-F1. Hold out entire articles and sources. Audit overlap with AraFacts before using the two resources across training and test. If highlighting exact rhetorical spans, annotate and evaluate span locations; snippet-level classification alone does not validate those highlights.

### Source and access:

R10.pdf, pp. 3-7, Tables 1-8. [Paper](https://aclanthology.org/2023.arabicnlp-1.44/). [Task data and scorers](https://araieval.gitlab.io/).

### Simplifed:

Paper 10, ArAIEval, helps us analyze persuasion techniques in Arabic text—for example, emotionally loaded language or attacks on someone’s reputation. This supports the media-analysis part of our platform.

**Imagine two descriptions of the same hypothetical decision:**

> **Article A:** “The council approved the new policy.”

> **Article B:** “The council approved this disastrous policy.”

Both might report the same decision, but “disastrous” adds emotionally loaded language. The paper’s persuasion task asks a model to recognize techniques like this.

Persuasive wording does not automatically make a statement false. We should display persuasion analysis separately from factual verification.

#### What tasks does the paper cover?

**It has two main areas:**

- **Persuasion detection:** Does a passage contain a persuasion technique, and which techniques?
- **A separate tweet-classification task:** Categories include hate speech, offensive content, rumor, and spam. Although the authors call this disinformation detection, these categories do not all mean that a factual claim is false.

#### What data did they use?

For persuasion, they used 3,189 text snippets, combining tweets and news paragraphs. Some news pages were obtained through links in AraFacts.

Three annotators independently identified techniques, then consolidators reviewed their annotations. The taxonomy contains 23 techniques, including loaded language, name calling, exaggeration, and appeals to fear.

The separate tweet task used approximately 20,000 COVID-related tweets collected in February–March 2020.

#### How did the systems work?

Most teams fine-tuned pretrained Arabic language models, such as AraBERT and MARBERT, to classify the text. Some used ensembles or training methods intended to handle uneven numbers of examples across techniques.

#### Were the results good?

- **Detecting whether any persuasion technique exists:** best micro-F1 of 76.34%.
- **Identifying the specific techniques:** best micro-F1 of 56.66%.

The second task was considerably harder. That winning system’s macro-F1 was only 21.56%, indicating much weaker performance when rare techniques receive equal importance.

So the results are useful, but we should be cautious about confidently identifying uncommon techniques.

#### How can we use it and improve further?

After grouping articles about the same event, our platform could show which persuasion techniques appear in each article.

We could improve this feature by collecting more examples of rare techniques and evaluating each technique separately. If we want to highlight exact words, we must also evaluate whether those highlights are correct; a passage-level label alone does not prove that.

#### The connection between the ten papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which articles cover the same event?
- **ArAIEval:** What persuasion techniques appear in the Arabic coverage of that event?

---

## R11 - AraBERT (2020)

### What and why:

Pretrains an Arabic-specific language model to improve language understanding where multilingual models have limited Arabic data and vocabulary capacity. It is a reusable foundation for downstream models, not a complete misinformation detector.

### Data and how they obtained it:

Approximately 24 GB and 70 million sentences after sentence deduplication, from scraped Arabic news plus the 1.5 Billion Words Arabic Corpus and OSIAN. Evaluation uses hotel/book reviews and tweets for sentiment, ANERcorp for named entities, and ARCD plus translated SQuAD examples for question answering. ANERcorp has about 16,500 entity mentions. ARCD has 1,395 questions; training also uses 2,966 translated SQuAD question-answer examples.

### How it works:

A BERT-base-style bidirectional encoder learns masked-word prediction and next-sentence prediction, then is fine-tuned for each downstream task. Arabic-specific segmentation with Farasa is used in v1; v0.1 has no such segmentation. Pretraining took four days on the paper's TPUv2-8 setup, which is a historical compute report rather than a requirement for us to reproduce from scratch.

### Results and whether they are good:

On ANERcorp, v0.1 achieved 84.2 macro-F1 versus multilingual BERT's 78.4 and the previous system's 81.7. On ASTD sentiment, v1 reached 92.6% accuracy versus mBERT 80.1%. Gains were not universal: on ARCD, v1's F1 was 62.7 versus 61.3, while exact match fell to 30.6 from 34.2. On LABR, v1's 86.7% remained below the cited previous 87.5%. It is a strong historical Arabic foundation, with task-dependent gains and no direct factual-verification result.

### What we can take:

Fine-tune an existing checkpoint for Arabic entity extraction, stance, or persuasion. Entity extraction can help align people, organizations, and places in retrieved evidence. Match the preprocessing to the checkpoint version.

### How we can improve and test it:

Compare Arabic-specific and multilingual encoders on the same journalism task, including dialectal/code-switched inputs where relevant. For extraction, measure entity-level F1 and inspect transliteration/spelling variants; for evidence tasks, use claim/evidence labels. Sentiment is an attitude signal, not evidence of truth or political bias. Continued news-domain pretraining is an optional experiment only if simpler fine-tuning leaves a measurable gap.

### Source and access:

R11.pdf, pp. 2-5, especially Table 1. [Paper](https://aclanthology.org/2020.osact-1.2/). [Models and preprocessing](https://github.com/aub-mind/arabert).

### Simplified:

Paper 11, AraBERT, gives us a pretrained model for understanding Arabic text. We can train it further for specific jobs in our platform, such as recognizing names, identifying persuasion techniques, or comparing a claim with an article. `C:\Work\Uni\Grad-Project\papers\R11.pdf`

**Consider this invented sentence:**

> “افتتحت جامعة النور مكتبة جديدة في القاهرة”

**An entity-recognition model built using AraBERT could identify:**

- **جامعة النور:** an organization.
- **القاهرة:** a location.

That information could help our platform distinguish articles about different universities and connect coverage mentioning the same organization.

#### Why did the authors develop it?

Arabic has complex word forms, prefixes, and suffixes. The authors wanted an Arabic-specific model with more suitable training data and vocabulary than multilingual BERT provided.

AraBERT first learns general Arabic patterns. We then fine-tune it, meaning we train it further using labeled examples of our particular task.

For example, persuasion examples from ArAIEval, paper 10, could teach it to recognize persuasion techniques.

#### What data did they use?

They pretrained it on approximately 24 GB of Arabic text containing 70 million sentences, after removing duplicate sentences.

The text came from scraped news websites and existing Arabic news collections. They then evaluated it on separate datasets for:

- Sentiment analysis.
- Named entity recognition.
- Question answering.

#### How does it learn?

During pretraining, some words are hidden and the model learns to predict them using the surrounding text. It also learns relationships between sentences.

The authors tested versions with different Arabic word-segmentation approaches, so preprocessing must match the chosen model version.

#### Were the results good?

**In the original 2020 study:**

- Entity recognition reached 84.2 F1, compared with multilingual BERT’s 78.4.
- On one sentiment dataset, accuracy reached 92.6%, compared with 80.1%.

These were useful improvements, but gains varied by task. They do not establish that AraBERT can independently detect false claims.

#### How can we use it and improve further?

We could fine-tune an existing checkpoint for Arabic entity extraction, stance, or persuasion. We would not need to repeat its original pretraining.

For our project, useful experiments include testing news from unseen outlets, handling spelling variations and dialects, and checking whether extracted entities actually improve retrieval or event grouping.

Unlike E5 and BGE-M3, AraBERT was not specifically trained as a ready-to-use semantic search model. Its clearest role here is as a foundation for task-specific Arabic understanding.

#### The connection between the eleven papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which articles cover the same event?
- **ArAIEval:** What persuasion techniques appear in Arabic coverage?
- **AraBERT:** What Arabic language model can we train to perform extraction and classification tasks inside our platform?

---

## R12 - GigaBERT: Arabic-English information extraction (2020)

### What and why:

Tests whether a bilingual model tailored to Arabic and English improves information extraction and English-to-Arabic transfer. The actual paper title is An Empirical Study of Pre-trained Transformers for Arabic Information Extraction.

### Data and how they obtained it:

Pretraining combines English/Arabic Gigaword news, Wikipedia, and Arabic OSCAR web text. Later versions use 10.4 billion tokens: 6.1 billion English and 4.3 billion Arabic, with Arabic upsampling. Synthetic code-switching substitutes words/entities using PanLex, MUSE dictionaries, and Wikipedia parallel titles. Evaluation uses ACE 2005 for entity recognition, argument roles, and relations, and Universal Dependencies v1.4 for part-of-speech tagging. The paper lists the licensed LDC Gigaword resources; these are not assumed freely redistributable.

### How it works:

Train bilingual BERT variants with a custom 50,000-subword vocabulary, extend training to longer sequences, and continue training on partly code-switched text. Evaluate both supervised Arabic learning and zero-shot transfer: train with English task labels and test on Arabic. Table 3 reports checkpoint selection with English versus Arabic development data separately.

### Results and whether they are good:

GigaBERT-v4 achieved 84.1 Arabic supervised NER F1, compared with AraBERT 78.6, mBERT 72.9, and XLM-R-base 81.5 on this study's ACE setup. For English-to-Arabic NER transfer it scored 51.5 versus mBERT 30.8 using English-development checkpoint selection. This is a substantial historical transfer gain, but far below supervised Arabic performance. The larger XLM-R-large scored 84.8 on supervised Arabic NER, so GigaBERT did not beat every model in every setting. Do not compare these ACE scores directly with AraBERT's ANERcorp score.

### What we can take:

Use bilingual entity representations and test transfer when Arabic annotations are scarce. Dictionary/entity anchors and domain-adapted training are useful ideas; pretrained extraction models still need evaluation on our articles.

### How we can improve and test it:

Compare English-only transfer, translated training data, and a small amount of manually labeled Arabic data. Add entity linking/alias resolution to connect Arabic names and English transliterations. Measure Arabic extraction F1 plus the downstream change in event grouping and evidence retrieval. Test code-switch augmentation as a controlled addition rather than assuming it always improves natural Arabic.

### Source and access:

R12.pdf, pp. 2-5, Tables 1-3. [Paper](https://aclanthology.org/2020.emnlp-main.382/). [Official model repository](https://github.com/lanwuwei/GigaBERT).

### Simplified:

Paper 12, GigaBERT, studies how a model trained specifically on Arabic and English can extract information from both languages—and transfer learning from English to Arabic. This is useful when we have more labeled English examples than Arabic ones. `C:\Work\Uni\Grad-Project\papers\R12.pdf`

**Imagine we train a model using English examples such as:**

> “Ahmed Ali visited Cairo.”

The training labels identify Ahmed Ali as a person and Cairo as a location.

**We then test whether it can recognize the corresponding types of information in Arabic:**

> “زار أحمد علي القاهرة.”

**The important question is:**

Can learning from labeled English text help us understand Arabic text without requiring equally large Arabic training datasets?

#### What does the model extract?

**The paper evaluates several tasks, including:**

- Identifying people, organizations, and locations.
- Identifying grammatical roles.
- Extracting relationships between entities.
- Identifying participants’ roles in events.

These components could help our platform understand who did what, where, and involving whom.

#### What data did the authors use?

**Later GigaBERT versions were pretrained on approximately 10.4 billion tokens, combining:**

- Arabic and English Gigaword news.
- Wikipedia.
- Arabic web text from OSCAR.

They also created synthetic mixed-language sentences by replacing some words or entities using bilingual dictionaries. This gave the model additional connections between Arabic and English.

For evaluation, they used labeled resources including ACE 2005 for information extraction.

#### Were the results good?

**On the paper’s Arabic entity-recognition test:**

- GigaBERT-v4 achieved 84.1 F1.
- Multilingual BERT achieved 72.9 F1.

When trained using English task labels and tested on Arabic, GigaBERT achieved 51.5 F1, compared with multilingual BERT’s 30.8 under the specified checkpoint-selection setting.

That is a substantial transfer improvement. However, 51.5 remains far below its supervised Arabic result, so English training does not remove the need for Arabic annotations.

Also, these scores use a different dataset from paper 11’s AraBERT results; we should not directly compare those numbers.

#### How can we use it and improve further?

We could test bilingual extraction for our news articles, particularly when Arabic labels are limited.

**A useful experiment would compare:**

1. Training with English labels only.
2. Translating training examples into Arabic.
3. Adding a small, manually labeled Arabic set.

We could then measure whether better extraction improves event grouping and evidence retrieval. Connecting “أحمد علي” and “Ahmed Ali” to the same individual would still require an additional entity-linking step.

#### The connection between the twelve papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which articles cover the same event?
- **ArAIEval:** What persuasion techniques appear in Arabic coverage?
- **AraBERT:** What model can support Arabic extraction and classification?
- **GigaBERT:** How can we extract information across Arabic and English, especially by transferring learning from English to Arabic?

---

## R13 - NoMIRACL (2024)

### What and why:

Tests whether a model recognizes that retrieved passages do or do not contain an answer. Retrieval-augmented generation can still fail when a passage looks relevant but lacks the necessary information.

### Data and how they obtained it:

Human judgments over Wikipedia retrieval in 18 languages, including Arabic and English, following MIRACL's native-speaker query generation and relevance assessment. One subset contains only judged non-relevant passages; the other contains at least one relevant passage. Counts are 24,825 non-relevant queries and 31,232 relevant queries across development/test (our sums of Table 1). Experiments cap each language/subset at 250 queries where available, so model results do not use every dataset item.

### How it works:

Give an LLM a query and up to ten passages and ask for a binary answer-presence decision. Hallucination rate is the false-positive rate on the non-relevant subset; error rate is the false-negative rate on the relevant subset. These measure two distinct failure modes. This is mainly relevance assessment, not scoring the factual accuracy of a generated long answer.

### Results and whether they are good:

The paper reports rates above 88% on the non-relevant subset for models such as Llama-2 and Orca-2. Its discussion reports mean hallucination rates of 17.4% for Mixtral-8x7B, 26.8% for Llama-3-8B, and 35.5% for GPT-4. However, Mixtral and Llama-3-8B have over 40% error on answerable cases. Lower hallucination alone can therefore reflect excessive rejection. These are concerning results under this setup and useful evidence that attaching retrieved text does not guarantee grounded output; they are not universal hallucination rates for the models today.

### What we can take:

Build and evaluate an explicit insufficient-evidence decision. Keep absence of evidence distinct from evidence of falsity, and report both unsupported decisions and unnecessary abstention.

### How we can improve and test it:

Build Arabic-English examples with topically similar irrelevant passages, then tune an evidence-sufficiency threshold on development data. Compare bilingual passages with translated passages. Report false assertions on insufficient cases, false abstentions on answerable cases, and accuracy among accepted decisions versus coverage. Inspect truncation: a passage labeled relevant in full may lose its answer if only its first 375 tokens are supplied, as in this paper's setup.

### Source and access:

R13.pdf, pp. 1, 3-7, Table 1 and Figures 5-7. [Paper](https://aclanthology.org/2024.findings-emnlp.730/). [Dataset and evaluation](https://github.com/project-miracl/nomiracl).

### Simplified:

Paper 13, NoMIRACL, asks whether a model can recognize when the retrieved passages do not contain the answer. For our platform, this helps us decide when to say “There is insufficient evidence.”

**Imagine a user asks:**

> “Did the university cancel tomorrow’s exams?”

**Our search retrieves:**

> “The university opened a new library yesterday.”

The passage mentions the correct university, but it does not answer the question. The system should recognize that gap instead of treating a related passage as sufficient evidence.

#### Why is this important?

Even a good search system sometimes returns irrelevant or incomplete results. Giving those passages to a language model does not guarantee that it will recognize their limitations.

Also, insufficient retrieved evidence means we cannot determine the answer from these passages. It does not mean the claim is false or that no answer exists anywhere.

#### What data did the authors use?

NoMIRACL contains human-judged questions and Wikipedia passages across 18 languages, including Arabic and English.

**It has two kinds of examples:**

- **Answer available:** at least one retrieved passage contains information needed to answer the question.
- **Answer unavailable:** none of the retrieved passages contains that information.

It builds on the retrieval and annotation approach used in MIRACL, paper 6.

#### How did they test the models?

They gave each model a question and up to ten passages, then asked it to decide whether an answer was present.

**They measured two mistakes:**

| Mistake | Example |
| --- | --- |
| False acceptance | The model says an answer exists, although the passages lack it. |
| False rejection | The model says it cannot answer, although a passage contains the answer. |

This mainly tests recognition of answer availability, rather than whether a long generated explanation is factually correct.

#### Were the results good?

Some tested models incorrectly claimed an answer was present in more than 88% of the non-relevant cases.

Other models made fewer false acceptances but rejected too many answerable cases. Therefore, simply making a system more cautious does not necessarily make it better.

These are concerning results under the paper’s experimental setup—not general hallucination rates for every use of those models.

#### How can we use it and improve further?

We could add an evidence-sufficiency check before our platform produces an assessment.

We would test it on Arabic and English passages, including misleading near-matches. We should measure both unsupported decisions and unnecessary refusals. A system that always says “I don’t know” would avoid unsupported answers but be unhelpful.

#### The connection between the thirteen papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which articles cover the same event?
- **ArAIEval:** What persuasion techniques appear in Arabic coverage?
- **AraBERT:** What model can support Arabic extraction and classification?
- **GigaBERT:** How can learning transfer between English and Arabic?
- **NoMIRACL:** Do the retrieved passages actually contain enough information to answer, or should our platform acknowledge insufficient evidence?

---

## R14 - SemEval-2023 Task 3: genre, framing, and persuasion

### What and why:

Separates three aspects of media presentation: article genre, the issues/frames emphasized, and rhetorical persuasion techniques. This supports comparing how outlets describe an event beyond whether the underlying facts are correct.

### Data and how they obtained it:

2,049 news documents in nine languages, collected from 2020 to mid-2022 across mainstream and alternative sources using aggregators and source lists. Arabic is absent. About 40 annotators, each native or near-native, labeled the material; each article received at least two annotations and curator consolidation. Genre and frames are document-level; persuasion was annotated in spans and evaluated as paragraph labels. Six languages have training data and three are surprise test languages.

### How it works:

Genre classification chooses opinion, reporting, or satire. Framing is multilabel classification over 14 generic frames, such as economic, security, or health. Persuasion is multilabel classification over 23 techniques in six broad groups. Systems mainly fine-tuned multilingual transformers, with translation, ensembles, and threshold tuning among the variants.

### Results and whether they are good:

English genre macro-F1 reached 0.784 versus baseline 0.288, while best English persuasion micro-F1 was only 0.376. Tables 13-14 report best nine-language means of 0.576 micro-F1 for framing and 0.420 for persuasion, versus baseline 0.356 and 0.223 respectively. Genre is relatively strong; fine-grained framing/persuasion remain difficult. Pre-consolidation agreement was alpha 0.342. Curator review helps produce labels, but does not demonstrate that all subjectivity has disappeared.

### What we can take:

Reuse the task separation and annotation guidelines, then combine the persuasion taxonomy with Arabic work such as R10. Show specific frames and techniques rather than converting them into an unsupported universal bias score.

### How we can improve and test it:

Pilot an Arabic frame annotation scheme with bilingual annotators, adjudicate disagreements, and assess agreement before scaling. Compare multilingual transfer, translated training, and native Arabic labels. Evaluate macro/micro-F1 by language and frame, including rare classes and held-out sources. Connect analysis to same-event clusters and validate any evidence highlights separately. Translation alone is not proof that culturally dependent framing labels transfer correctly.

### Source and access:

R14.pdf, pp. 1-6 and 8-10, especially Tables 3, 9, and 13-14. [Paper](https://aclanthology.org/2023.semeval-1.317/). [Task resources and guidelines](https://propaganda.math.unipd.it/semeval2023task3/). Genre aggregate Table 12 is ambiguous: its caption says macro, but some values match averages of the per-language micro scores. We use the unambiguous per-language genre result instead.

### Simplified:

Paper 14 helps us analyze how news is presented: what kind of article it is, which aspects it emphasizes, and which persuasion techniques it uses. This supports comparing Arabic and English coverage of the same event.

**Imagine two hypothetical articles about a new factory:**

> **Article A:** “New factory will create 2,000 jobs.”

> **Article B:** “Residents raise concerns about pollution from the new factory.”

They may cover the same event, while emphasizing different aspects. The first emphasizes economic effects; the second emphasizes health or environmental concerns.

That choice of emphasis is called framing. Different framing does not automatically mean either article is false.

#### What does the paper detect?

**It separates three tasks:**

| Task | Question | Possible output |
| --- | --- | --- |
| Genre | What kind of article is this? | Reporting, opinion, or satire |
| Framing | Which aspects of the issue receive attention? | Economic, security, health, political, etc. |
| Persuasion | Which rhetorical techniques appear? | Loaded language, exaggeration, appeals to fear, etc. |

An article can contain several frames and persuasion techniques simultaneously.

#### What data did they use?

The authors collected 2,049 news documents in nine languages, published between 2020 and mid-2022.

**Annotators labeled:**

- Genre and frames at the article level.
- Persuasion techniques in text spans, subsequently evaluated as paragraph labels.

Each document received at least two annotations followed by curator review. The dataset covers 14 frames and 23 persuasion techniques.

Arabic is absent, so we cannot assume a model trained on this dataset will work reliably on Arabic news.

#### How did the systems work?

This is another shared-task overview. Most systems fine-tuned pretrained language models. Some combined training data across languages, translated examples, or combined several models.

#### Were the results good?

- Best English genre performance reached 78.4% macro-F1.
- Best nine-language average framing performance reached 57.6% micro-F1.
- Best nine-language average persuasion performance reached 42.0% micro-F1.

These involve different tasks and metrics, so they are not directly comparable. They nevertheless show that detailed framing and persuasion analysis remains challenging.

Human annotators also disagreed, making clear guidelines and careful review important.

#### How can we use it and improve further?

After paper 9’s event-grouping step, our platform could show how different outlets emphasize different aspects of the same event.

We could use this paper’s guidelines for English and develop a carefully reviewed Arabic annotation set. Paper 10 provides Arabic persuasion examples, which could support the persuasion component; it does not automatically supply Arabic framing labels.

Our improvement would be tested through Arabic–English evaluation and human review, rather than turning these predictions into an unsupported overall “bias score.”

#### The connection between the fourteen papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which articles cover the same event?
- **ArAIEval:** What persuasion techniques appear in Arabic coverage?
- **AraBERT:** What model can support Arabic extraction and classification?
- **GigaBERT:** How can learning transfer between English and Arabic?
- **NoMIRACL:** Do the retrieved passages contain an answer?
- **Genre, framing, and persuasion:** How do outlets present the same event, and which aspects do they emphasize?

---

## R15 - ARAFA (supplied 2026 arXiv version)

### What and why:

Generates a large Arabic claim-evidence dataset automatically to reduce the cost of manual annotation. It targets Modern Standard Arabic Wikipedia verification, not live web retrieval.

### Data and how they obtained it:

Around 10,000 cleaned Arabic Wikipedia pages were sampled and chunked at paragraph boundaries. GPT-4o generated claims with extracted evidence and mutated 50,000 supported claims using date, quantity, attribute, relation, and context changes. Claude Sonnet 3.5 validated relations or changed labels to Not Enough Information. Final counts: 181,976 pairs, including 111,303 supported, 42,109 refuted, and 28,564 NEI. A 400-pair sample, drawn from supported/refuted generation candidates, received expert ground truth and additional human review.

### How it works:

Generation and validation use structured prompting and entity/coreference checks. The final dataset trains two encoders and two generative models to classify a claim supplied together with evidence. This tests evidence interpretation; it does not test finding evidence from the web. Most evidence is short: 89.4% consists of one sentence.

### Results and whether they are good:

AraModernBert-Base-V1.0 reached the best macro-F1, 77%, at 81% accuracy. AraBERTv2 and Qwen2.5-7B reached 82% accuracy. The best-macro encoder's class F1s were 87% supported, 91% refuted, and only 52% NEI. Validator accuracy was 86% on supported/NEI and 88% on refuted/NEI sample subsets; these are not guarantees that every dataset label is correct. The scale is useful and results promising, but external validity and evaluation separation need checking.

### What we can take:

Consider it supplemental training for Arabic claim-evidence inference. Its controlled mutations also suggest challenge cases for dates, numbers, and entity relations. Keep independent naturally occurring claims for final evaluation.

### How we can improve and test it:

Explicitly exclude the human test sample and related Wikipedia pages/claim mutations from training before splitting. Section 4 says the complete dataset was split 90/10 for training/validation and the same 400-pair sample was used for testing; it does not clearly document prior exclusion. This is a potential leakage issue, not proof leakage occurred. The sample also helped select the validator. Audit raw data and split code, create a fresh human-labeled test, compare real-data-only versus synthetic-plus-real training, and focus on NEI recall. A validation rule that considers linguistic quality when assigning NEI may mix awkward language with genuinely insufficient evidence; audit those labels separately.

### Source and access:

R15.pdf, pp. 1-3 and 5-13, especially Section 4 and Tables 5-6. [Supplied version's paper record](https://arxiv.org/abs/2609.25833). [Author repository](https://github.com/chriskhalil/ARAFA), which links an [archived dataset](https://doi.org/10.5281/zenodo.16762969). The repository also cites a 2025 Research Square preprint; the supplied PDF is arXiv v1 dated 22 September 2026. Match paper and dataset versions before reproduction.

### Simplified:

Paper 15, ARAFA, creates a large Arabic training dataset using AI-generated claims and evidence. For our project, it could help train a model to distinguish whether a passage supports a claim, contradicts it, or provides insufficient information.

**Consider this hypothetical evidence:**

> “جامعة النور تأسست عام 1995.”

> “Al-Noor University was established in 1995.”

**We can create different training examples from it:**

| Claim | Relationship to that evidence |
| --- | --- |
| “The university was established in 1995.” | Supported |
| “The university was established in 2005.” | Refuted |
| “The university has 5,000 students.” | Not Enough Information |

The third claim is not necessarily false. The passage simply does not tell us its student population.

#### Why did the authors create it?

Arabic has fewer large claim–evidence datasets than English. Manually writing and checking thousands of examples is expensive, so the authors investigated automated generation.

#### What data did they use, and how did they create it?

**They sampled approximately 10,000 Arabic Wikipedia pages and used this process:**

1. **Generate supported claims:** GPT-4o creates claims paired with evidence from the Wikipedia text.
2. **Modify some claims:** Change dates, quantities, attributes, or relationships to create contradictory examples.
3. **Validate the relationships:** Claude Sonnet 3.5 checks the pairs and can assign insufficient-information labels.

**The final dataset contains 181,976 claim–evidence pairs:**

- 111,303 supported.
- 42,109 refuted.
- 28,564 insufficient information.

Humans also reviewed a 400-example sample to assess quality.

Unlike AraFacts, these are primarily generated examples, rather than naturally circulating claims collected from fact-checkers.

#### Were the results good?

The best tested model, AraModernBERT, achieved 77% macro-F1 and 81% accuracy.

However, its insufficient-information F1 was only 52%. It was considerably better at recognizing support and contradiction than recognizing missing evidence.

The results are promising for training, but they do not establish reliable real-news verification. The model receives the evidence as input; it does not have to find that evidence itself.

There is also an evaluation concern: the paper does not clearly document whether its 400-example human test sample was excluded from training. We should check that separation before relying on the reported score; this does not prove that overlap occurred.

#### How can we use it and improve further?

We could use ARAFA as supplementary training data for Arabic evidence interpretation.

**A useful experiment would compare training with:**

- Real, human-labeled examples only.
- ARAFA only.
- ARAFA followed by real-data fine-tuning.

All three should be tested on the same independent, human-checked collection of real news claims, with special attention to insufficient evidence.

#### The connection between the fifteen papers is:

- **AraFacts:** Has someone already checked this claim?
- **AraStance:** How does this article relate to the claim?
- **AVeriTeC:** What evidence do we need, and what conclusion does it justify?
- **FEVER:** Can we provide evidence supporting our verdict?
- **SemEval claim retrieval:** Can we find an existing fact-check across languages?
- **MIRACL:** How well does our search rank relevant passages?
- **BGE-M3:** What model can perform multilingual semantic search?
- **Multilingual E5:** What alternative search model should we compare?
- **News article similarity:** Which articles cover the same event?
- **ArAIEval:** What persuasion techniques appear in Arabic coverage?
- **AraBERT:** What model can support Arabic extraction and classification?
- **GigaBERT:** How can learning transfer between English and Arabic?
- **NoMIRACL:** Do the retrieved passages contain an answer?
- **Genre, framing, and persuasion:** How do outlets present the same event?
- **ARAFA:** Can generated Arabic claim–evidence examples improve our model’s ability to recognize support, contradiction, and insufficient information?

---
