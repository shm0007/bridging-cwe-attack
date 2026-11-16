# Third-party notices and licensing status

The author authorized distribution of these research artifacts. No new blanket license for author-created datasets, documentation, examples, or fine-tuned contributions has been selected. Final license metadata remains an author decision. This statement does not replace upstream licenses or grant rights to third-party content beyond its existing terms.

## MITRE sources

CAPEC, CWE, and ATT&CK names, identifiers, definitions, and mappings are attributed to The MITRE Corporation. Preserve applicable copyright and terms when redistributing copies. Source-specific notices and grant text are recorded in `notices/MITRE_TERMS.md`.

- CAPEC: https://capec.mitre.org/about/termsofuse.html
- CWE: https://cwe.mitre.org/about/termsofuse.html
- ATT&CK: https://attack.mitre.org/resources/terms-of-use/

## CISA ICS and generated symptoms

Source: CISA's publicly accessible CSAF OT collection, https://github.com/cisagov/CSAF . Source identifiers, URLs, dates, and existing metadata remain unmodified. `notices/CISA_CSAF_README.md` is copied verbatim from the source checkout; its original relative links may require navigation in the upstream repository. CISA's republication notice is retained. Public-source collection is not described as a universal public-domain dedication.

Symptoms were generated with Meta-Llama-3.1-8B-Instruct for evaluation only. Attribute Meta and the Llama 3.1 model: https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct ; agreement: https://github.com/meta-llama/llama-models/blob/main/models/llama3_1/LICENSE . Llama weights are not included. Frozen symptom queries were not used to train the released encoders. No separate broad redistribution investigation was conducted; existing notices are retained.

## Encoder models

E5 base: https://huggingface.co/intfloat/e5-base-v2 (upstream card: MIT).
MPNet base: https://huggingface.co/sentence-transformers/all-mpnet-base-v2 (upstream card: Apache-2.0).
Model packages retain the original inherited base-model card under `notices/upstream_model_card.md`. It describes the base model and is not a claim that its benchmark values were measured for the fine-tuned checkpoint. Required upstream copyright/license obligations remain applicable. Final fine-tuned contribution licensing is not newly assigned by this staging process.

## Funding

The paper acknowledges partial support from NSF SFS #2043250. Opinions, findings, conclusions, and recommendations are those of the authors and do not necessarily reflect the views of the National Science Foundation.
