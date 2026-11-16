# Download, integrity, and evaluation usage

## Obtain data

Clone [GitHub repository](https://github.com/shm0007/bridging-cwe-attack) or download its source archive. The Hugging Face dataset has identical files under `data/`. The following commands use the final repository identifiers. The example uses `main`; for a recorded evaluation, pin an observed remote commit after publication:

```python
from huggingface_hub import snapshot_download
from datasets import load_dataset

root = snapshot_download(repo_id="ehsan0007/cwe-attack-retrieval-benchmark",
                         repo_type="dataset", revision="main")
# Pair files retain their original split field. This loads records, not a new training split.
pairs = load_dataset("csv", data_files={"records": root +
    "/data/capec/capec_cwe_positive_pairs.csv"}, split="records")
train = pairs.filter(lambda row: row["split"] == "train")
validation = pairs.filter(lambda row: row["split"] == "val")
test = pairs.filter(lambda row: row["split"] == "test")
```

Alternatively use `load_dataset("ehsan0007/cwe-attack-retrieval-benchmark", "capec_cwe_pairs", split="records")`. Use the dataset-card configurations; the `records` split is a storage view, not a scientific training designation. Candidate TXT files should be downloaded as files and parsed according to the protocol, not treated as one candidate per line.

## Exact CISA evaluation view

```python
from pathlib import Path
import pandas as pd

data = Path("data")  # GitHub checkout root/data
frozen = pd.read_csv(data / "cisa_ics/cisa_ics_cwe_eval_unique_text_v1.csv")
audit = pd.read_csv(data / "cisa_ics/frozen_row_audit.csv")
positive = pd.read_csv(data / "capec/capec_cwe_positive_pairs.csv")
as_bool = lambda s: s.astype(str).str.lower().isin(["true", "1", "yes"])
usable = set(audit.loc[as_bool(audit.usable_for_both_query_types), "row_index"])
mask = as_bool(frozen.low_leakage_eval_eligible) & frozen.index.isin(usable)
evaluation = frozen.loc[mask].copy()  # in-memory view only; do not overwrite frozen data
seen = set(positive.loc[positive.split == "train", "target_id"])
evaluation["seen_target"] = evaluation.cwe_id.isin(seen)
assert len(evaluation) == 10592
assert evaluation.seen_target.sum() == 7511
assert (~evaluation.seen_target).sum() == 3081
# Raw queries: evaluation.vulnerability_summary; symptoms: evaluation.llama_query
```

## Obtain and load checkpoints

Download each model repository separately; keep every tokenizer/config/module file with its weights. Use a pinned model commit for a recorded evaluation. The minimum conceptual dependencies are PyTorch, Transformers, Sentence Transformers, and safetensors. Historical checkpoint metadata record Sentence Transformers 5.3.0, Transformers 4.56.1, and PyTorch 2.8.0+cu128. Install a compatible CPU or CUDA build for your platform; those historical versions are not a newly validated environment lock.

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

## Integrity

From either dataset repository root, run `sha256sum -c data/manifests/SHA256SUMS`. All original dataset bytes are unchanged. `data/manifests/files.csv` records original paths, release paths, byte counts, row counts, and SHA-256. Do not edit or sort a frozen CSV before checking its original positional index audit.

Model repositories have their own `SHA256SUMS`, including tokenizer/config files and documentation. A staging-only verification record documents whether an actual runtime load succeeded. Full experimental Python source, optimization runs, and full intermediate predictions are not distributed. This supports checkpoint evaluation and independent reimplementation; it is not a complete original-code training reproduction package.

## Verified runtime smoke test

All four staged checkpoints were loaded offline on CPU with SentenceTransformer using the existing research environment (Python 3.10.12, PyTorch 2.8.0+cu128, Transformers 4.56.1, Sentence Transformers 5.3.0, and safetensors 0.6.2). Each encoded seven paper-relevant inputs into a finite (7, 768) matrix. L2 norms passed within 1e-5; the maximum observed deviation was about 1.2e-7. E5 query:/passage: token prefixes and MPNet unprefixed formatting were explicitly checked. These are loading/encoding smoke tests, not reruns of the full reported experiments. No packages were installed or upgraded, no training occurred, and immutable checkpoint hashes were unchanged.
