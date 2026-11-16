# Data dictionary

CSV files retain original strings, empty cells, Boolean serialization, and column order. Pipe-separated set fields are strings, not JSON arrays. IDs must remain strings. Blank fields mean missing/not recorded; do not impute labels. Hugging Face CSV inference may map blank cells to null and boolean-like fields to bool; this changes only the in-memory representation, not the released file bytes. The TXT corpora have structured labelled records, not one document per physical line.

## data/capec/capec_master.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| name | Original CAPEC name. |
| abstraction | CAPEC abstraction metadata. |
| status | CAPEC status metadata. |
| description | Original CAPEC description. |
| retrieval_text | CAPEC name and description joined for retrieval. |
| alternate_terms | Original CAPEC alternate-term metadata. |
| likelihood_of_attack | CAPEC qualitative likelihood metadata. |
| typical_severity | CAPEC typical severity metadata. |
| related_cwes | Pipe-separated explicit related CWE labels. |
| attack_ids | Pipe-separated ATT&CK labels. |
| attack_names | Pipe-separated names associated with explicit ATT&CK mappings. |
| related_capec_ids | Related CAPEC metadata; not hierarchy-propagated training labels. |
| is_attack_view | Original CAPEC ATT&CK-view membership flag. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |
| has_cwe | Whether CAPEC has explicit CWE mappings. |
| has_attack | Whether CAPEC has explicit ATT&CK mappings. |
| has_both | Whether CAPEC has both relation families. |

## data/capec/capec_cwe_relationships.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| capec_name | Original supporting CAPEC name. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |

## data/capec/capec_attack_relationships.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| capec_name | Original supporting CAPEC name. |
| attack_id | ATT&CK technique/subtechnique identifier. |
| attack_name | Original mapped ATT&CK name. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |

## data/capec/capec_split_seed42.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| split | Fixed scientific partition: train, val, or test. |
| has_cwe | Whether CAPEC has explicit CWE mappings. |
| has_attack | Whether CAPEC has explicit ATT&CK mappings. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |
| stratum | Fixed split stratum derived from has_cwe, has_attack, and is_ics. |

## data/capec/capec_cwe_positive_pairs.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| split | Fixed scientific partition: train, val, or test. |
| task | Relation target taxonomy: cwe or attack. |
| query_text | Exact CAPEC retrieval query text. |
| target_id | Positive CWE or ATT&CK target identifier. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |

## data/capec/capec_attack_positive_pairs.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| split | Fixed scientific partition: train, val, or test. |
| task | Relation target taxonomy: cwe or attack. |
| query_text | Exact CAPEC retrieval query text. |
| target_id | Positive CWE or ATT&CK target identifier. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |

## data/capec/pair_statistics.csv

| Column | Meaning |
|---|---|
| split | Fixed scientific partition: train, val, or test. |
| capec_ids | Distinct CAPEC query count in a task/partition. |
| positive_pairs | Positive-pair record count in a task/partition. |
| unique_targets | Distinct target count in a task/partition. |
| task | Relation target taxonomy: cwe or attack. |

## data/cwe_attack/capec_cwe_attack_relationships.csv

| Column | Meaning |
|---|---|
| capec_id | CAPEC pattern identifier; join key to global split and master table. |
| capec_name | Original supporting CAPEC name. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| attack_id | ATT&CK technique/subtechnique identifier. |
| is_ics | Original CAPEC ICS-view membership flag; not an evaluation candidate filter. |
| evidence_type | Original relationship-induction provenance label. |

## data/cwe_attack/capec_unique_cwe_attack_pairs.csv

| Column | Meaning |
|---|---|
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| attack_id | ATT&CK technique/subtechnique identifier. |
| evidence_count | Number of distinct CAPEC patterns supporting an endpoint pair. |
| capec_ids | Pipe-separated supporting CAPEC identifiers. |
| any_ics | Whether any supporting CAPEC pattern has ICS membership. |

## data/cwe_attack/retained_strict_novel_edges.csv

| Column | Meaning |
|---|---|
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| attack_id | ATT&CK technique/subtechnique identifier. |
| edge_status | Recorded retained or removed edge category. |

## data/cwe_attack/removed_train_overlap_edges.csv

| Column | Meaning |
|---|---|
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| attack_id | ATT&CK technique/subtechnique identifier. |
| edge_status | Recorded retained or removed edge category. |

## data/cwe_attack/removed_validation_overlap_edges.csv

The source is a one-byte newline with no header or records. It records zero additional validation-overlap exclusions. Do not load it as a CSV table without handling the empty-file case.

## data/cisa_ics/cisa_ot_cwe_benchmark.csv

| Column | Meaning |
|---|---|
| source | Recorded source label. |
| source_file | Original public source artifact path. |
| advisory_id | Original advisory identifier. |
| advisory_title | Original advisory title. |
| initial_release_date | Source initial publication date. |
| current_release_date | Source revision/publication date. |
| critical_infrastructure_sector | Source sector metadata. |
| countries_deployed | Source deployment-location metadata. |
| company_headquarters | Source vendor-location metadata. |
| cisa_web_url | Original advisory URL. |
| advisory_summary | Advisory-level text; not the raw query field. |
| cve_id | Original CVE identifier. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| cwe_name | Source-reported weakness name; reference metadata, never model input. |
| vulnerability_summary | Original CISA vulnerability description used for the raw query view. |
| cvss_version | Source CVSS version. |
| cvss_score | Source CVSS score. |
| cvss_severity | Source CVSS severity label. |
| cvss_vector | Source CVSS vector. |
| vulnerability_attack_ids | ATT&CK references extracted at vulnerability level; not CWE retrieval gold. |
| vulnerability_attack_references | Source vulnerability-level ATT&CK reference metadata. |
| advisory_attack_ids | Advisory-level ATT&CK reference identifiers; not endpoint relevance labels. |
| advisory_attack_references | Advisory-level source references. |

## data/cisa_ics/cisa_ot_cwe_benchmark_llama_queries.csv

| Column | Meaning |
|---|---|
| source | Recorded source label. |
| source_file | Original public source artifact path. |
| advisory_id | Original advisory identifier. |
| advisory_title | Original advisory title. |
| initial_release_date | Source initial publication date. |
| current_release_date | Source revision/publication date. |
| critical_infrastructure_sector | Source sector metadata. |
| countries_deployed | Source deployment-location metadata. |
| company_headquarters | Source vendor-location metadata. |
| cisa_web_url | Original advisory URL. |
| advisory_summary | Advisory-level text; not the raw query field. |
| cve_id | Original CVE identifier. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| cwe_name | Source-reported weakness name; reference metadata, never model input. |
| vulnerability_summary | Original CISA vulnerability description used for the raw query view. |
| cvss_version | Source CVSS version. |
| cvss_score | Source CVSS score. |
| cvss_severity | Source CVSS severity label. |
| cvss_vector | Source CVSS vector. |
| vulnerability_attack_ids | ATT&CK references extracted at vulnerability level; not CWE retrieval gold. |
| vulnerability_attack_references | Source vulnerability-level ATT&CK reference metadata. |
| advisory_attack_ids | Advisory-level ATT&CK reference identifiers; not endpoint relevance labels. |
| advisory_attack_references | Advisory-level source references. |
| llama_query | Frozen Llama-generated symptom-oriented query. |
| llama_generation_status | Generation completion/status label. |
| llama_generation_error | Recorded generation error, if any; not model prediction data. |
| llama_model | Recorded generation model identifier. |
| llama_prompt_version | Generation prompt version identifier. |

## data/cisa_ics/cisa_ics_cwe_eval_all_v1.csv

| Column | Meaning |
|---|---|
| source | Recorded source label. |
| source_file | Original public source artifact path. |
| advisory_id | Original advisory identifier. |
| advisory_title | Original advisory title. |
| initial_release_date | Source initial publication date. |
| current_release_date | Source revision/publication date. |
| critical_infrastructure_sector | Source sector metadata. |
| countries_deployed | Source deployment-location metadata. |
| company_headquarters | Source vendor-location metadata. |
| cisa_web_url | Original advisory URL. |
| advisory_summary | Advisory-level text; not the raw query field. |
| cve_id | Original CVE identifier. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| cwe_name | Source-reported weakness name; reference metadata, never model input. |
| vulnerability_summary | Original CISA vulnerability description used for the raw query view. |
| cvss_version | Source CVSS version. |
| cvss_score | Source CVSS score. |
| cvss_severity | Source CVSS severity label. |
| cvss_vector | Source CVSS vector. |
| vulnerability_attack_ids | ATT&CK references extracted at vulnerability level; not CWE retrieval gold. |
| vulnerability_attack_references | Source vulnerability-level ATT&CK reference metadata. |
| advisory_attack_ids | Advisory-level ATT&CK reference identifiers; not endpoint relevance labels. |
| advisory_attack_references | Advisory-level source references. |
| llama_query | Frozen Llama-generated symptom-oriented query. |
| llama_generation_status | Generation completion/status label. |
| llama_generation_error | Recorded generation error, if any; not model prediction data. |
| llama_model | Recorded generation model identifier. |
| llama_prompt_version | Generation prompt version identifier. |
| generation_ok | Successful status and nonempty generated query. |
| identifier_leak | At least one screened taxonomy/CVE identifier appears in generated query. |
| identifier_leak_types | Pipe-separated identifier flag types. |
| exact_gold_cwe_name_overlap | Exact gold-name overlap diagnostic. |
| exact_gold_cwe_name_matches | Matched CWE ID/name strings. |
| substantial_gold_cwe_name_overlap | Gold-name token overlap diagnostic at threshold 0.60. |
| substantial_gold_cwe_name_matches | Matched CWE names and overlap scores. |
| explicit_weakness_phrase | Diagnostic weakness terminology flag; not independent eligibility exclusion. |
| explicit_weakness_phrases | Matched diagnostic terminology. |
| source_word_count | Token count in cleaned source description. |
| query_word_count | Token count in generated query. |
| source_query_jaccard | Source/query token-set intersection over union. |
| query_token_containment_in_source | Fraction of query tokens present in source. |
| eligible_strict | Generation success and no explicit identifier flags. |
| no_exact_name_eval_eligible | Strict eligibility plus no exact gold-CWE-name overlap. |
| low_leakage_eval_eligible | No-exact-name eligibility plus no substantial gold-name-token overlap. |
| freeze_version | Frozen benchmark version label. |

## data/cisa_ics/cisa_ics_cwe_eval_unique_cve_text_v1.csv

| Column | Meaning |
|---|---|
| source | Recorded source label. |
| source_file | Original public source artifact path. |
| advisory_id | Original advisory identifier. |
| advisory_title | Original advisory title. |
| initial_release_date | Source initial publication date. |
| current_release_date | Source revision/publication date. |
| critical_infrastructure_sector | Source sector metadata. |
| countries_deployed | Source deployment-location metadata. |
| company_headquarters | Source vendor-location metadata. |
| cisa_web_url | Original advisory URL. |
| advisory_summary | Advisory-level text; not the raw query field. |
| cve_id | Original CVE identifier. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| cwe_name | Source-reported weakness name; reference metadata, never model input. |
| vulnerability_summary | Original CISA vulnerability description used for the raw query view. |
| cvss_version | Source CVSS version. |
| cvss_score | Source CVSS score. |
| cvss_severity | Source CVSS severity label. |
| cvss_vector | Source CVSS vector. |
| vulnerability_attack_ids | ATT&CK references extracted at vulnerability level; not CWE retrieval gold. |
| vulnerability_attack_references | Source vulnerability-level ATT&CK reference metadata. |
| advisory_attack_ids | Advisory-level ATT&CK reference identifiers; not endpoint relevance labels. |
| advisory_attack_references | Advisory-level source references. |
| llama_query | Frozen Llama-generated symptom-oriented query. |
| llama_generation_status | Generation completion/status label. |
| llama_generation_error | Recorded generation error, if any; not model prediction data. |
| llama_model | Recorded generation model identifier. |
| llama_prompt_version | Generation prompt version identifier. |
| generation_ok | Successful status and nonempty generated query. |
| identifier_leak | At least one screened taxonomy/CVE identifier appears in generated query. |
| identifier_leak_types | Pipe-separated identifier flag types. |
| exact_gold_cwe_name_overlap | Exact gold-name overlap diagnostic. |
| exact_gold_cwe_name_matches | Matched CWE ID/name strings. |
| substantial_gold_cwe_name_overlap | Gold-name token overlap diagnostic at threshold 0.60. |
| substantial_gold_cwe_name_matches | Matched CWE names and overlap scores. |
| explicit_weakness_phrase | Diagnostic weakness terminology flag; not independent eligibility exclusion. |
| explicit_weakness_phrases | Matched diagnostic terminology. |
| source_word_count | Token count in cleaned source description. |
| query_word_count | Token count in generated query. |
| source_query_jaccard | Source/query token-set intersection over union. |
| query_token_containment_in_source | Fraction of query tokens present in source. |
| eligible_strict | Generation success and no explicit identifier flags. |
| no_exact_name_eval_eligible | Strict eligibility plus no exact gold-CWE-name overlap. |
| low_leakage_eval_eligible | No-exact-name eligibility plus no substantial gold-name-token overlap. |
| freeze_version | Frozen benchmark version label. |

## data/cisa_ics/cisa_ics_cwe_eval_unique_text_v1.csv

| Column | Meaning |
|---|---|
| source | Recorded source label. |
| source_file | Original public source artifact path. |
| advisory_id | Original advisory identifier. |
| advisory_title | Original advisory title. |
| initial_release_date | Source initial publication date. |
| current_release_date | Source revision/publication date. |
| critical_infrastructure_sector | Source sector metadata. |
| countries_deployed | Source deployment-location metadata. |
| company_headquarters | Source vendor-location metadata. |
| cisa_web_url | Original advisory URL. |
| advisory_summary | Advisory-level text; not the raw query field. |
| cve_id | Original CVE identifier. |
| cwe_id | Reported CWE identifier; single gold label in the CISA benchmark. |
| cwe_name | Source-reported weakness name; reference metadata, never model input. |
| vulnerability_summary | Original CISA vulnerability description used for the raw query view. |
| cvss_version | Source CVSS version. |
| cvss_score | Source CVSS score. |
| cvss_severity | Source CVSS severity label. |
| cvss_vector | Source CVSS vector. |
| vulnerability_attack_ids | ATT&CK references extracted at vulnerability level; not CWE retrieval gold. |
| vulnerability_attack_references | Source vulnerability-level ATT&CK reference metadata. |
| advisory_attack_ids | Advisory-level ATT&CK reference identifiers; not endpoint relevance labels. |
| advisory_attack_references | Advisory-level source references. |
| llama_query | Frozen Llama-generated symptom-oriented query. |
| llama_generation_status | Generation completion/status label. |
| llama_generation_error | Recorded generation error, if any; not model prediction data. |
| llama_model | Recorded generation model identifier. |
| llama_prompt_version | Generation prompt version identifier. |
| generation_ok | Successful status and nonempty generated query. |
| identifier_leak | At least one screened taxonomy/CVE identifier appears in generated query. |
| identifier_leak_types | Pipe-separated identifier flag types. |
| exact_gold_cwe_name_overlap | Exact gold-name overlap diagnostic. |
| exact_gold_cwe_name_matches | Matched CWE ID/name strings. |
| substantial_gold_cwe_name_overlap | Gold-name token overlap diagnostic at threshold 0.60. |
| substantial_gold_cwe_name_matches | Matched CWE names and overlap scores. |
| explicit_weakness_phrase | Diagnostic weakness terminology flag; not independent eligibility exclusion. |
| explicit_weakness_phrases | Matched diagnostic terminology. |
| source_word_count | Token count in cleaned source description. |
| query_word_count | Token count in generated query. |
| source_query_jaccard | Source/query token-set intersection over union. |
| query_token_containment_in_source | Fraction of query tokens present in source. |
| eligible_strict | Generation success and no explicit identifier flags. |
| no_exact_name_eval_eligible | Strict eligibility plus no exact gold-CWE-name overlap. |
| low_leakage_eval_eligible | No-exact-name eligibility plus no substantial gold-name-token overlap. |
| freeze_version | Frozen benchmark version label. |

## data/cisa_ics/frozen_row_audit.csv

| Column | Meaning |
|---|---|
| row_index | Zero-based position in the unchanged primary unique-text frozen CSV. |
| usable_for_both_query_types | Availability of both query views and a usable candidate target. |
| gold_ids | Pipe-separated original reference CWE IDs. |
| present_gold_ids | Reference IDs present in the candidate set. |
| missing_gold_ids | Reference IDs absent from the candidate set. |
| exclusion_reason | Recorded reason an original row is not usable. |

## data/cisa_ics/excluded_rows.csv

| Column | Meaning |
|---|---|
| row_index | Zero-based position in the unchanged primary unique-text frozen CSV. |
| usable_for_both_query_types | Availability of both query views and a usable candidate target. |
| gold_ids | Pipe-separated original reference CWE IDs. |
| present_gold_ids | Reference IDs present in the candidate set. |
| missing_gold_ids | Reference IDs absent from the candidate set. |
| exclusion_reason | Recorded reason an original row is not usable. |
