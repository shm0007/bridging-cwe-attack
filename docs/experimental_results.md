# Published experimental results

Values below are paper-reported results cross-checked against saved summaries. They are not measurements rerun while packaging. OTS is off the shelf. Table I FT is task-specific; Table II CWE-FT is CWE-specific; Joint denotes shared two-relation training.

## Paper Table I: held-out CAPEC retrieval

69 CWE queries and 28 ATT&CK queries. Values rounded to three decimals.

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

## Paper Table II: unseen-target CISA ICS evaluation

3,081 queries selected from the primary low-leakage benchmark, all evaluated against 939 CWE candidates.

| Encoder | Training | Raw MRR | Raw Hit@10 | Symptom MRR | Symptom Hit@10 |
|---|---|---|---|---|---|
| E5-base-v2 | OTS | 0.233 | 0.361 | 0.166 | 0.295 |
| E5-base-v2 | CWE-FT | 0.255 | 0.409 | 0.140 | 0.272 |
| E5-base-v2 | Joint | 0.315 | 0.455 | 0.179 | 0.327 |
| MPNet | OTS | 0.099 | 0.195 | 0.113 | 0.227 |
| MPNet | CWE-FT | 0.241 | 0.435 | 0.158 | 0.309 |
| MPNet | Joint | 0.275 | 0.464 | 0.178 | 0.326 |

## Section V-C and Figure 2: strict endpoint retrieval

96 retained relevance edges. Values rounded to four decimals.

| Encoder | Direction | Condition | Queries | MRR | Recall@10 | Hit@10 | nDCG@10 |
|---|---|---|---|---|---|---|---|
| E5-base-v2 | attack_to_cwe | joint_bridge | 34 | 0.1551 | 0.2320 | 0.3824 | 0.1293 |
| E5-base-v2 | cwe_to_attack | joint_bridge | 49 | 0.1082 | 0.0891 | 0.1837 | 0.0688 |
| E5-base-v2 | attack_to_cwe | off_the_shelf | 34 | 0.1470 | 0.1703 | 0.2941 | 0.1066 |
| E5-base-v2 | cwe_to_attack | off_the_shelf | 49 | 0.0570 | 0.0653 | 0.1224 | 0.0407 |
| MPNet | attack_to_cwe | joint_bridge | 34 | 0.1888 | 0.3248 | 0.4706 | 0.1802 |
| MPNet | cwe_to_attack | joint_bridge | 49 | 0.1190 | 0.1912 | 0.3061 | 0.1128 |
| MPNet | attack_to_cwe | off_the_shelf | 34 | 0.1863 | 0.1752 | 0.2941 | 0.1249 |
| MPNet | cwe_to_attack | off_the_shelf | 49 | 0.0969 | 0.1197 | 0.2245 | 0.0782 |

MPNet CWE→ATT&CK Recall@10 increases by 0.0714 (about 59.7% relative): reported 95% bootstrap interval [0.0204, 0.1327], Cohen's dz 0.350. nDCG@10 increases from 0.0782 to 0.1128: interval [0.0046, 0.0695], dz 0.298. E5 CWE→ATT&CK MRR increases by about 89.9% and Hit@10 by 50.0%. These are relative changes, not percentage-point gains.

## Figures 1, 3, and 4

Figure 1 is the conceptual CAPEC-mediated learning framework. Figure 3 shows OTS/Joint gold-CWE rank distributions by encoder, raw/symptom formulation, and seen/unseen exposure; lower rank is better. Figure 4 shows MPNet unseen-target Hit@k for k=1…100, comparing OTS, CWE-FT, and Joint for both query formulations. Complete original plotting code and per-query prediction outputs are not included. The datasets and released models support independent reevaluation.

## Section VI case studies

For MPNet on CVE-2018-5452 (CWE-121, absent as a positive training target), raw-query ranks are OTS 123, CWE-FT 30, Joint 1. Symptom-query ranks are 552, 822, 869. For CVE-2015-3965, symptom-query ranks are 18, 25, 2. These saved-result ranks illustrate both successful transfer and failure when a rewrite omits distinguishing mechanism information. They do not establish robust performance on all indirect queries.

## Private provenance references

Table I: `results/capec_finetune_comparison/summary.csv`, `results/capec_bm25/summary.csv`, `results/capec_transformer_ots/summary.csv`.
Table II: `results/cisa_ics_final/cisa_ics_generalization_summary.csv`, subset `low_leakage`, exposure `unseen_in_capec_train`.
Endpoint: `results/cwe_attack_novel_edges_strict_control/strict_novel_edge_summary.csv`, subset `all_strict_novel_edges`, conditions `off_the_shelf` and `joint_bridge`.
These paths identify audit sources; the release does not require access to that private directory to interpret its datasets, model cards, or protocol.
