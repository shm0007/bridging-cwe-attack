# Dataset construction

## Sources and provenance

- CAPEC: https://capec.mitre.org/ ; paper-reported version 3.9. Explicit Related Weaknesses and Taxonomy Mappings are extracted without propagating mappings through the hierarchy.
- CWE: https://cwe.mitre.org/ ; paper-reported version 4.17. Exact formatted candidate text is included; original XML/export revision is not available in the audited project.
- MITRE ATT&CK: https://attack.mitre.org/ . The exact experimental text corpus is retained. An upstream STIX version/commit is not recorded in the supplied artifact.
- CISA CSAF OT collection: https://github.com/cisagov/CSAF/tree/develop/csaf_files/OT . Exact downloaded commit is not recorded. Per-record advisory identifiers, source paths, dates, URLs, and CWE/CVE metadata remain in the files.
- Symptom generator: `meta-llama/Meta-Llama-3.1-8B-Instruct`; prompt version `cisa_ics_symptom_v1`.

## CAPEC preparation and partitions

The source export loader repairs a stray quote in the ID header and extra empty trailing CSV fields. CAPEC retrieval text joins name and description. Every explicit CAPEC–CWE or CAPEC–ATT&CK association is a separate positive pair. Identifier lists are pipe-separated. ICS/view metadata are labels attached to CAPEC patterns; they do not restrict the final candidate corpus.

A single global partition is made by CAPEC identifier using seed 42, nominal 70%/15%/15% ratios, and strata defined by CWE relation availability, ATT&CK relation availability, and ICS membership. Rounding and small-stratum allocation produce the counts below. Groups of size one go to training; size two allocates one training and one test identifier. This fixed split propagates to both pair tables. Use the existing split assignments, not a new random partition.

| Partition | Global CAPEC IDs | CWE pairs / queries | ATT&CK pairs / queries |
|---|---:|---:|---:|
| Training | 428 | 843 / 314 | 188 / 123 |
| Validation | 92 | 188 / 67 | 37 / 26 |
| Test | 95 | 181 / 69 | 46 / 28 |

## CISA source benchmark

The extractor collects OT vulnerability descriptions and reported CWE labels, requires both text and a CWE, and deduplicates by advisory ID, CVE ID, CWE ID, and vulnerability text. The resulting labelled benchmark has 13,029 records and 442 distinct CWE values. The release includes that complete source benchmark and its complete generated-query companion.

The generator receives only the vulnerability description, not the reference CWE. Its instructions preserve supported observations, access conditions, ICS context, and consequences; avoid taxonomy identifiers and formal weakness classifications; and request a concise diagnostic query. Generation uses `do_sample=False`, at most 160 new tokens, repetition penalty 1.05, and input truncation at 4,096 tokens. The implementation uses a chat template and stores model, status, error, and prompt-version fields. The frozen output, rather than a fresh generation with a current model revision, defines this benchmark.

## Screening and freezing

Generation status must be `ok` with nonempty text. Screening checks explicit CWE, CVE, ATT&CK, and CAPEC identifiers; exact gold-CWE name overlap; and substantial name-token overlap. For substantial overlap, common stopwords and name tokens of length at most two are removed, and the fraction of remaining name tokens found in the query is compared with 0.60. Explicit weakness-phrase flags, word counts, source/query Jaccard similarity, and containment are retained as diagnostics.

`eligible_strict` requires generation success and no identifier flags. `no_exact_name_eval_eligible` additionally excludes exact name matches. `low_leakage_eval_eligible` additionally excludes substantial name matches; explicit phrase flags do not independently remove a row.

The eligible frozen all-record view has 13,027 rows. Unique source text is selected after sorting by source text and CVE ID, keeping the first record: 11,246 rows. A separate unique CVE/source-text view has 11,681 rows. These are preserved, including their original ordering and flags.

## Primary paper membership and exposure

The unique-text view has 10,606 low-leakage-flagged rows. Thirteen CWE-592 rows and one CWE-533 row have targets absent from the 939-entry corpus. Excluding those yields exactly 10,592 paper instances. `frozen_row_audit.csv` and `excluded_rows.csv` retain zero-based positional row indices into the unmodified unique-text CSV.

Seen means the gold CWE occurs as a positive target in the CAPEC–CWE pair table with `split == "train"`. There are 286 distinct training targets. The primary benchmark contains 7,511 seen-target and 3,081 unseen-target queries. Both query views use the same membership and complete 939-entry CWE candidate corpus. No CISA examples are training or validation data.
