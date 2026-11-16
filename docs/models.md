# Released fine-tuned encoders

| Resource | Final publication destination |
|---|---|
| Paper | Not yet supplied |
| GitHub | [GitHub repository](https://github.com/shm0007/bridging-cwe-attack) |
| Dataset | [Hugging Face dataset](https://huggingface.co/datasets/ehsan0007/cwe-attack-retrieval-benchmark) |
| Joint E5 | [Joint E5 model](https://huggingface.co/ehsan0007/cwe-attack-retrieval-e5) |
| Joint MPNet | [Joint MPNet model](https://huggingface.co/ehsan0007/cwe-attack-retrieval-mpnet) |
| CWE-FT E5 | [CWE-FT E5 model](https://huggingface.co/ehsan0007/cwe-retrieval-e5) |
| CWE-FT MPNet | [CWE-FT MPNet model](https://huggingface.co/ehsan0007/cwe-retrieval-mpnet) |

Four independently packaged encoders are released: Joint E5 (selected epoch 1), Joint MPNet (2), CWE-FT E5 (8), and CWE-FT MPNet (2). Joint models implement the proposed shared relation supervision; CWE-only models support the Table II ablation. OTS models are available from their original publishers. Other task-specific Table I models and all exploratory/shuffled models are not included.

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

Joint E5 base is `intfloat/e5-base-v2`; Joint MPNet base is `sentence-transformers/all-mpnet-base-v2`. CWE-only models share those bases. Each has tokenizer files, Transformer config/weights, mean Pooling config, and Normalize module declaration. Normalization has no learned weights. Preserve all these files when downloading. The model repository README describes selected epoch and evaluation subset; its retained inherited base card is attribution only.

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

All four staged checkpoints were loaded offline on CPU with SentenceTransformer using the existing research environment (Python 3.10.12, PyTorch 2.8.0+cu128, Transformers 4.56.1, Sentence Transformers 5.3.0, and safetensors 0.6.2). Each encoded seven paper-relevant inputs into a finite (7, 768) matrix. L2 norms passed within 1e-5; the maximum observed deviation was about 1.2e-7. E5 query:/passage: token prefixes and MPNet unprefixed formatting were explicitly checked. These are loading/encoding smoke tests, not reruns of the full reported experiments. No packages were installed or upgraded, no training occurred, and immutable checkpoint hashes were unchanged.
