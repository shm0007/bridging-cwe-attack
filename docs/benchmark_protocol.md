# Benchmark and evaluation protocol

## Text construction and candidate sets

Use 939 CWE IDs and 1,062 unique ATT&CK attack-pattern IDs. The original text files are not regenerated or reformatted. CWE IDs are normalized to `CWE-N`; ATT&CK IDs retain `TNNNN[.NNN]`.

For CWE, split into blank-line-separated blocks. Read the labelled `ID:`, `Name:`, `Description:`, and `Extended Description:` lines. Candidate text joins name, description, and extended description with spaces, omitting empty values. The historical parsers read the value on the labelled line; unlabelled continuation lines are not concatenated. Do not silently substitute a full-document parser or prepend IDs.

For ATT&CK, the saved evaluators read blank-line-delimited labelled records; retain only records with `Type: attack-pattern`; join labelled name and description values. Unlabelled continuation lines are not concatenated. The supplied file also contains malware and course-of-action records; these are not candidates. Keep candidate order as read. CAPEC query text comes directly from `query_text` in the positive-pair CSVs.

E5 prefixes query text with `query: ` and candidate text with `passage: `, including endpoint retrieval in either direction. MPNet has no prefixes. The saved prompt-default configuration is empty, so E5 formatting must be explicit. Encode normalized embeddings, compute dot products (cosine similarity), and rank the full candidate corpus. The original evaluators use NumPy argsort on descending scores. Truncation follows the saved model configurations: 512 E5 tokens, 384 MPNet tokens.

## Tasks and relevance

- CAPEC→CWE: one held-out query per CAPEC ID, all valid associated CWE labels relevant; 69 test queries.
- CAPEC→ATT&CK: analogous, 28 test queries.
- Strict endpoint retrieval: form `(w,a)` when a CAPEC pattern links to both. Group relation evidence by the fixed CAPEC split. Evaluate `E_test − (E_train ∪ E_val)`, not all pairs from held-out CAPEC alone. Test induces 125 pairs; 29 overlap with training; validation removes zero additional pairs. The saved 96 retained edges yield 49 CWE→ATT&CK and 34 ATT&CK→CWE queries. Group retained edges by query endpoint for relevance labels. Supporting train/validation relations remain available for auditing; they are not direct training pairs.
- External CISA→CWE: use the unique-text frozen CSV, low-leakage flag and candidate availability. Evaluate original `vulnerability_summary` and frozen `llama_query` on identical 10,592 instances. The reference `cwe_id` is the single relevant target.

## Metrics

Report macro averages across queries. MRR is reciprocal rank of the first relevant candidate (zero if none). Recall@k is relevant candidates retrieved in the first k divided by the number of relevant candidates. Hit@k is one if any relevant candidate is retrieved. Binary nDCG@k uses gain 1 per relevant item, discount `1/log2(rank+1)`, and normalization by the ideal ranking containing min(k, number of gold labels) relevant items. CISA single-label Recall@k equals Hit@k. Use the full ranking for MRR, not MRR@10. Report relative change as `(joint − baseline)/baseline`, distinguishing it from an absolute percentage-point difference.

## Statistical comparisons

The paper describes 10,000 paired bootstrap resamples, 95% intervals, 10,000 paired permutation samples, seed 42, and Cohen's dz. Pair by query and benchmark membership, not merely row order from separately filtered outputs. CISA's saved statistics apply Holm correction to three metrics within each comparison/model/query-type/exposure family. Endpoint saved p-values are uncorrected; their confidence intervals and effect sizes are retained as reported facts in the documentation, not relabelled. This release does not recompute statistical results.

## Baselines

OTS means the pretrained encoder. CWE-FT means the selected CAPEC→CWE checkpoint; Joint means the selected shared two-relation checkpoint. Table I additionally includes BM25, mean-pooled OTS BERT and SecureBERT, MiniLM, and BGE. BM25 uses lowercase alphanumeric tokenization, without stemming or stopword removal, and BM25Okapi library defaults. These additional baseline weights and the original experimental source are not distributed here.
