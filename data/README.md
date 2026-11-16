# Dataset inventory

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

See [construction](../docs/dataset_construction.md), [column definitions](../docs/data_dictionary.md), [protocol](../docs/benchmark_protocol.md), and [notices](../THIRD_PARTY_NOTICES.md). All files are original copies; views and partitions are documented rather than regenerated.
