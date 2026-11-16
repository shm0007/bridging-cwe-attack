# Bridging Cybersecurity Taxonomies: CAPEC-Mediated Representation Learning for CWE–ATT&CK Retrieval

**Research datasets, fixed benchmarks, and four fine-tuned retrieval encoders associated with the camera-ready ICSS paper.**

**Authors:** Shamim Ehsan, Aaron Zambran, Elizabeth Martin, Alejandra De La Pena, Thinley Jurmi, Angel Coronel, Salamah Salamah, M. Shahriar Hossain, Monika Akbar. **Affiliation:** The University of Texas at El Paso.


## Paper, data, and model repositories

| Resource | Final publication destination |
|---|---|
| Paper | Not yet supplied |
| GitHub | [GitHub repository](https://github.com/shm0007/bridging-cwe-attack) |
| Dataset | [Hugging Face dataset](https://huggingface.co/datasets/ehsan0007/cwe-attack-retrieval-benchmark) |
| Joint E5 | [Joint E5 model](https://huggingface.co/ehsan0007/cwe-attack-retrieval-e5) |
| Joint MPNet | [Joint MPNet model](https://huggingface.co/ehsan0007/cwe-attack-retrieval-mpnet) |
| CWE-FT E5 | [CWE-FT E5 model](https://huggingface.co/ehsan0007/cwe-retrieval-e5) |
| CWE-FT MPNet | [CWE-FT MPNet model](https://huggingface.co/ehsan0007/cwe-retrieval-mpnet) |


This repository distributes the complete approved research datasets and benchmark views, detailed protocols, and links to four separately hosted models. Full original experimental source code, intermediate predictions, and large model weights are not included in GitHub. The Hugging Face dataset contains byte-identical dataset files. The paper PDF is not redistributed here; insert its final public URL above.

## Research overview

CWE describes software and hardware weaknesses; MITRE ATT&CK describes adversarial behavior; CAPEC describes attack patterns with curated relationships to both. The paper asks whether supervision through CAPEC can induce useful compatibility between CWE and ATT&CK without direct endpoint-pair training.

A shared bi-encoder learns from CAPEC–CWE and CAPEC–ATT&CK descriptions. Joint training uses symmetric multi-positive contrastive losses, treating all known in-batch relations as positives. Each step combines one batch from each relation type with equal weights; the smaller stream is cycled. CAPEC is removed at endpoint-retrieval inference. CWE-only comparison checkpoints use the identified single-relation implementation's directional CAPEC→CWE multi-positive loss.

The contributions are (1) CAPEC-mediated cross-taxonomy representation learning; (2) evaluation on endpoint relationships absent from training and validation; and (3) external transfer to CISA ICS vulnerability descriptions, including unseen CWE targets and frozen symptom-oriented rewrites. CISA data are used exclusively for evaluation.


## Release organization

```text
README.md, CITATION.cff, THIRD_PARTY_NOTICES.md
data/capec/          descriptions, explicit relations, positive pairs, global split
data/cwe_attack/     exact candidate inputs, induced relations, strict edge audits
data/cisa_ics/       full source/generated benchmark, frozen views, row audits
data/manifests/      exact source/release paths, counts, sizes, SHA-256
docs/               construction, schemas, models, protocol, results, limitations
notices/            preserved source attribution and terms
huggingface/        dataset/model repository map
```

## Sources and dataset construction

The paper reports CAPEC 3.9 and CWE 4.17. Explicit CAPEC relationships provide supervision; no hierarchy propagation creates extra positive labels. CAPEC query text combines pattern name and description. Exact original candidate text files are preserved, producing 939 CWE and 1,062 ATT&CK attack-pattern candidates under the documented parsers. Taxonomy snapshot commits are not recorded and are not invented.

CISA data come from the public CSAF OT advisory collection. The labelled source benchmark contains 13,029 vulnerability records spanning 442 CWE values. Meta-Llama-3.1-8B-Instruct produced frozen symptom-oriented rewrites from descriptions without reference CWE input. Generation was deterministic (`do_sample=False`, maximum 160 new tokens, repetition penalty 1.05). See [construction](docs/dataset_construction.md) for extraction, deduplication, generation, and screening details, and [notices](THIRD_PARTY_NOTICES.md) for attribution.

### Dataset files and schemas

| File | Records | Bytes | Purpose |
|---|---|---|---|
| data/capec/capec_master.csv | 615 | 657115 | CAPEC descriptions, retrieval text, explicit labels and view metadata |
| data/capec/capec_cwe_relationships.csv | 1212 | 68807 | Explicit CAPEC–CWE relationships |
| data/capec/capec_attack_relationships.csv | 271 | 25288 | Explicit CAPEC–ATT&CK relationships |
| data/capec/capec_split_seed42.csv | 615 | 23589 | Authoritative global CAPEC-ID train/validation/test assignment |
| data/capec/capec_cwe_positive_pairs.csv | 1212 | 662715 | CWE positive pairs and exact CAPEC query text |
| data/capec/capec_attack_positive_pairs.csv | 271 | 136492 | ATT&CK positive pairs and exact CAPEC query text |
| data/capec/pair_statistics.csv | 6 | 178 | Task/partition counts |
| data/cwe_attack/capec_cwe_attack_relationships.csv | 616 | 53310 | CAPEC-supported endpoint relation evidence |
| data/cwe_attack/capec_unique_cwe_attack_pairs.csv | 567 | 19654 | Unique endpoint pairs and supporting CAPEC identifiers |
| data/cwe_attack/formatted_cwe_entries.txt | 939 | 643444 | Exact CWE candidate text input |
| data/cwe_attack/cleaned_attack_data.txt | 2130 | 1968795 | Exact input corpus: 2,130 objects; filter to 1,062 attack-pattern candidates |
| data/cwe_attack/retained_strict_novel_edges.csv | 96 | 5148 | 96 strict evaluation relevance edges |
| data/cwe_attack/removed_train_overlap_edges.csv | 29 | 1202 | 29 removed endpoint relations |
| data/cwe_attack/removed_validation_overlap_edges.csv | 0 | 1 | Zero additional exclusions; original one-byte empty file |
| data/cisa_ics/cisa_ot_cwe_benchmark.csv | 13029 | 9806514 | Complete 13,029-record labelled CISA source benchmark |
| data/cisa_ics/cisa_ot_cwe_benchmark_llama_queries.csv | 13029 | 13354643 | Complete source benchmark with frozen generated symptoms and generation metadata |
| data/cisa_ics/cisa_ics_cwe_eval_all_v1.csv | 13027 | 14588112 | Frozen generation/identifier-eligible records |
| data/cisa_ics/cisa_ics_cwe_eval_unique_cve_text_v1.csv | 11681 | 12968904 | Frozen unique CVE/source-text view |
| data/cisa_ics/cisa_ics_cwe_eval_unique_text_v1.csv | 11246 | 12514641 | Primary frozen unique-source-text view; filter for paper evaluation |
| data/cisa_ics/frozen_row_audit.csv | 11246 | 310796 | Zero-based row-index usability and candidate-availability audit |
| data/cisa_ics/excluded_rows.csv | 14 | 1022 | 14 rows excluded for unavailable gold CWE targets |


The [complete data dictionary](docs/data_dictionary.md) describes every CSV column, field conventions, missing values, and the empty validation-exclusion artifact. Relation lists are pipe-separated, partition labels are `train`, `val`, and `test`, and row indices refer to original unchanged ordering. Candidate corpora are labelled TXT blocks, not one candidate per line. The original ATT&CK file includes noncandidate object types; filter by `attack-pattern` as documented.

### Fixed CAPEC partitions

| Partition | Global CAPEC IDs | CWE pairs / queries | ATT&CK pairs / queries |
|---|---:|---:|---:|
| Training | 428 | 843 / 314 | 188 / 123 |
| Validation | 92 | 188 / 67 | 37 / 26 |
| Test | 95 | 181 / 69 | 46 / 28 |


A single seed-42 CAPEC-ID partition is shared across relation types. Nominal ratios are 70/15/15, stratified by relation availability and ICS membership; rounding and small strata explain exact counts. Task-specific query counts are smaller because not every CAPEC has both relation families. Do not randomly repartition positive pairs. Published training and validation labels are preserved exactly.

### Strict CWE↔ATT&CK benchmark

CAPEC induces an endpoint pair when it links to both concepts. From 125 test-induced pairs, remove 29 exposed through training and zero additional validation-only pairs. The retained 96 edges define 49 CWE→ATT&CK and 34 ATT&CK→CWE queries. All valid retained targets for a query are relevant. This tests unseen relations; endpoint concepts may still occur elsewhere in supervised data.

### CISA raw/symptom benchmark and exposure groups

The complete source and generated benchmark are included alongside all three frozen views. Primary evaluation uses the unique-source-text frozen view, not every record in every file. Its 11,246 rows include 10,606 low-leakage-flagged rows; 14 have unavailable CWE targets (13 CWE-592, one CWE-533). The final paper subset has **10,592 queries**. Both raw descriptions and frozen symptoms use identical membership.

Seen means the reference CWE appears as a positive training target in the CAPEC–CWE pair table; otherwise it is unseen. There are **7,511 seen-target** and **3,081 unseen-target** instances, evaluated over all 939 CWE candidates. CISA is evaluation-only. Screening excludes generation failure, explicit identifiers, exact gold-name overlap, and substantial gold-name-token overlap; weakness-phrase flags alone are diagnostic, not exclusion rules. The stored original row positions link to `frozen_row_audit.csv` and `excluded_rows.csv`.

## Released models and training

Four separate HF repositories host Joint E5, Joint MPNet, CWE-FT E5, and CWE-FT MPNet. Base models are `intfloat/e5-base-v2` and `sentence-transformers/all-mpnet-base-v2`. Selected epochs are 1, 2, 8, and 2 respectively. Joint checkpoints optimize symmetric multi-positive objectives for both relation types; CWE-FT checkpoints use the identified directional CAPEC→CWE objective. No direct endpoint pairs or CISA queries are used in optimization.

| Setting | Value |
|---|---|
| Seed | 42 |
| Maximum epochs | 8 |
| Optimizer | AdamW |
| Learning rate | 2 × 10⁻⁵ |
| Batch size | 24 positive-pair records before batch construction |
| Contrastive temperature | 0.05 |
| Gradient clipping | 1.0 |
| Joint loss weights | 0.5 CAPEC–CWE; 0.5 CAPEC–ATT&CK |
| Joint selection | Maximum mean validation MRR across the two CAPEC tasks |
| CWE-only selection | Maximum validation MRR on CAPEC→CWE |
| Similarity | Cosine; L2-normalized sentence embeddings |
| E5 maximum sequence length | 512 tokens |
| MPNet maximum sequence length | 384 tokens |
| Embedding dimension | 768 for both released encoder families |


Joint losses receive equal weights; the smaller stream is cycled. Model selection uses validation data only. Model cards give exact selected epochs, input formatting, checkpoint contents, and per-model evaluation values. Historical checkpoint metadata record Sentence Transformers 5.3.0, Transformers 4.56.1, and PyTorch 2.8.0+cu128; these are environment evidence, not an independently validated lock file.

## Evaluation settings

Tasks are held-out CAPEC→CWE/ATT&CK, strict endpoint retrieval, and external CISA→CWE. Rank the complete candidate corpus by cosine similarity. MRR uses the first relevant rank over the full ranking; Recall@k measures the fraction of relevant targets retrieved; Hit@k records any relevant hit; binary nDCG@k accounts for rank discounts and multiple valid targets. Average across queries. Single-label CISA Recall@k equals Hit@k. The [protocol](docs/benchmark_protocol.md) specifies candidate text construction, ordering, filtering, and metric definitions.

The paper uses 10,000 paired bootstrap resamples and 10,000 permutation samples, 95% intervals, and Cohen's dz. CISA saved statistics apply Holm correction within defined three-metric families; endpoint saved p-values are uncorrected. See [limitations](docs/known_limitations.md). This release describes original measurements and does not claim to have rerun them.

## Published results

### Table I: held-out CAPEC retrieval

69 CAPEC→CWE queries, 28 CAPEC→ATT&CK queries. FT here means separately adapted task-specific encoders, not joint checkpoints. BERT and SecureBERT are OTS mean-pooled baselines.

| Model | CWE MRR | CWE R@5 | CWE R@10 | CWE nDCG@10 | ATT&CK MRR | ATT&CK R@5 | ATT&CK R@10 | ATT&CK nDCG@10 |
|---|---|---|---|---|---|---|---|---|
| BM25 | 0.262 | 0.180 | 0.229 | 0.181 | 0.370 | 0.428 | 0.509 | 0.370 |
| BERT | 0.131 | 0.101 | 0.141 | 0.095 | 0.199 | 0.255 | 0.344 | 0.217 |
| SecureBERT | 0.175 | 0.132 | 0.188 | 0.142 | 0.330 | 0.296 | 0.403 | 0.322 |
| MiniLM | 0.340 | 0.279 | 0.325 | 0.264 | 0.452 | 0.635 | 0.695 | 0.486 |
| MiniLM-FT | 0.471 | 0.400 | 0.572 | 0.421 | 0.571 | 0.684 | 0.709 | 0.579 |
| BGE | 0.292 | 0.255 | 0.296 | 0.228 | 0.448 | 0.541 | 0.601 | 0.447 |
| BGE-FT | 0.475 | 0.411 | 0.515 | 0.407 | 0.525 | 0.673 | 0.751 | 0.554 |
| E5 | 0.253 | 0.225 | 0.260 | 0.207 | 0.381 | 0.517 | 0.564 | 0.391 |
| E5-FT | 0.558 | 0.478 | 0.569 | 0.487 | 0.498 | 0.678 | 0.761 | 0.544 |
| MPNet | 0.314 | 0.293 | 0.352 | 0.275 | 0.522 | 0.636 | 0.740 | 0.546 |
| MPNet-FT | 0.529 | 0.476 | 0.599 | 0.467 | 0.564 | 0.730 | 0.796 | 0.599 |


### Table II: CISA ICS queries with unseen CWE targets

3,081 low-leakage instances; raw and symptom views have matched membership.

| Encoder | Training | Raw MRR | Raw Hit@10 | Symptom MRR | Symptom Hit@10 |
|---|---|---|---|---|---|
| E5-base-v2 | OTS | 0.233 | 0.361 | 0.166 | 0.295 |
| E5-base-v2 | CWE-FT | 0.255 | 0.409 | 0.140 | 0.272 |
| E5-base-v2 | Joint | 0.315 | 0.455 | 0.179 | 0.327 |
| MPNet | OTS | 0.099 | 0.195 | 0.113 | 0.227 |
| MPNet | CWE-FT | 0.241 | 0.435 | 0.158 | 0.309 |
| MPNet | Joint | 0.275 | 0.464 | 0.178 | 0.326 |


MPNet Joint increases unseen-target raw Hit@10 from 19.5% OTS to 46.4%; CWE-FT reaches 43.5%. E5 symptom Hit@10 is 29.5% OTS, 27.2% CWE-FT, and 32.7% Joint. These distinguish the contribution of joint supervision from CWE-only adaptation.

For strict endpoint retrieval (Section V-C/Figure 2), MPNet CWE→ATT&CK Recall@10 rises from 0.1197 to 0.1912, about 59.7% relative, not 59.7 percentage points. Figure 3 compares gold-rank distributions; Figure 4 examines MPNet unseen-target Hit@k through k=100. [Experimental results](docs/experimental_results.md) provides endpoint metrics, intervals, case studies, and provenance of the saved summaries.

## Download and minimal usage

Data are directly available in `data/` on GitHub and identically in the HF dataset. See [download and reproduction instructions](docs/reproducibility.md) for pinned downloads, fixed split filtering, and exact in-memory CISA subset selection. Install PyTorch, Transformers, Sentence Transformers, and safetensors in a suitable environment; `datasets`, `huggingface_hub`, and pandas support the dataset examples.

```python
from sentence_transformers import SentenceTransformer

# Replace with a local model package directory or the final HF model ID.
model = SentenceTransformer("ehsan0007/cwe-attack-retrieval-e5")
q = model.encode(["query: A device halts after receiving crafted traffic."],
                 normalize_embeddings=True)
p = model.encode(["passage: <candidate text built using the documented parser>"],
                 normalize_embeddings=True)
scores = q @ p.T
print(scores)
```

Use the Joint MPNet or CWE-FT MPNet repository without E5 prefixes. CWE-FT E5 requires the same E5 prefixes. Do not pass a local directory name through a heuristic that guesses the family from its name: choose formatting explicitly from the base model metadata. Saved prompt defaults are empty.


From a dataset repository root run `sha256sum -c data/manifests/SHA256SUMS`. Every original dataset is copied byte-for-byte. Model packages have separate checksum manifests. No dataset regeneration, label edits, checkpoint conversions, or retraining are part of packaging.

## Reproducibility coverage and limitations

The strict endpoint test contains only 49 CWE→ATT&CK and 34 ATT&CK→CWE queries. Endpoint relevance is induced through CAPEC rather than independently annotated. Unseen endpoint relationships do not imply entirely unseen endpoint concepts. Training uses one fixed seed, so reported uncertainty does not measure variation across training runs. Generated symptoms are controlled rewrites, not independent analyst observations, and may retain weakness cues or omit distinguishing information. Scores rank candidates; they do not establish causality or a definitive vulnerability classification.

The exact candidate corpus, labels, query texts, partitions, and ordering are preserved. The raw ATT&CK text contains multiple object types; only `attack-pattern` entries are retrieval candidates. Candidate text must be constructed as specified in the protocol. Source snapshot identifiers are not recoverable for every taxonomy. CAPEC 3.9 and CWE 4.17 are the versions reported by the paper; the release does not invent missing source commit identifiers.

The paper describes a Weaviate indexing workflow; the saved-result evaluators use exact normalized embedding matrix products and NumPy sorting. The identified CWE-only trainer uses a directional loss while joint training uses symmetric losses. The symptom generator's implemented wording differs slightly from Appendix A. Frozen generated text is the authoritative evaluation input.

Explicit weakness phrases are diagnostic flags, not a separate low-leakage exclusion condition. CISA statistics apply Holm correction within comparison × model × query type × target-exposure families (three metrics). The endpoint significance outputs contain uncorrected permutation p-values; do not label them Holm-corrected. No experimental results were rerun to create this release. Full original training/evaluation source code is not included.


All four staged checkpoints were loaded offline on CPU with SentenceTransformer using the existing research environment (Python 3.10.12, PyTorch 2.8.0+cu128, Transformers 4.56.1, Sentence Transformers 5.3.0, and safetensors 0.6.2). Each encoded seven paper-relevant inputs into a finite (7, 768) matrix. L2 norms passed within 1e-5; the maximum observed deviation was about 1.2e-7. E5 query:/passage: token prefixes and MPNet unprefixed formatting were explicitly checked. These are loading/encoding smoke tests, not reruns of the full reported experiments. No packages were installed or upgraded, no training occurred, and immutable checkpoint hashes were unchanged.

## Citation, acknowledgments, and licensing

```bibtex
@misc{ehsan_bridging_cybersecurity_taxonomies,
  title = {Bridging Cybersecurity Taxonomies: CAPEC-Mediated Representation Learning for CWE–ATT\&CK Retrieval},
  author = {Ehsan, Shamim and Zambran, Aaron and Martin, Elizabeth and De La Pena, Alejandra and Jurmi, Thinley and Coronel, Angel and Salamah, Salamah and Hossain, M. Shahriar and Akbar, Monika}
}
```

The paper acknowledges partial support from **NSF SFS #2043250**. Opinions, findings, conclusions, and recommendations are those of the authors and do not necessarily reflect NSF views.

Original sources are MITRE CAPEC, CWE, ATT&CK, CISA CSAF OT, Meta Llama 3.1 (symptom generation), and the respective base encoder publishers. See [third-party notices](THIRD_PARTY_NOTICES.md). Existing attribution and inherited model notices are preserved. No new author-dataset, documentation, example-code, or fine-tuned-contribution license has been chosen; final author-material licensing remains an author decision; the destination URLs are fixed. Contact information is not added beyond the verified paper attribution.
