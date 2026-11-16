# ICSS Research Artifacts — Publication Report

**Final status: BLOCKED BEFORE UPLOAD. No publication success is claimed.**

Paper: Bridging Cybersecurity Taxonomies: CAPEC-Mediated Representation Learning for CWE–ATT&CK Retrieval.

## Destinations and remote status

| Resource | Destination | Visibility / existence | Upload |
|---|---|---|---|
| GitHub | [GitHub](https://github.com/shm0007/bridging-cwe-attack) | Existing repository; public/empty per author; not remotely verified | Not attempted; blocked at API preflight |
| HF dataset | [HF dataset](https://huggingface.co/datasets/ehsan0007/cwe-attack-retrieval-benchmark) | Not created by this workflow; remote state unverified | Not attempted; blocked at API preflight |
| Joint E5 | [Joint E5](https://huggingface.co/ehsan0007/cwe-attack-retrieval-e5) | Not created by this workflow; remote state unverified | Not attempted; blocked at API preflight |
| Joint MPNet | [Joint MPNet](https://huggingface.co/ehsan0007/cwe-attack-retrieval-mpnet) | Not created by this workflow; remote state unverified | Not attempted; blocked at API preflight |
| CWE-FT E5 | [CWE-FT E5](https://huggingface.co/ehsan0007/cwe-retrieval-e5) | Not created by this workflow; remote state unverified | Not attempted; blocked at API preflight |
| CWE-FT MPNet | [CWE-FT MPNet](https://huggingface.co/ehsan0007/cwe-retrieval-mpnet) | Not created by this workflow; remote state unverified | Not attempted; blocked at API preflight |

## Blocking issue

GitHub CLI could not connect to `api.github.com`. Hugging Face CLI reported a name-resolution failure for `huggingface.co` (`Name or service not known`). Thus authenticated account identity, remote repository state/default branch, card rendering, and uploaded inventories could not be verified. No repository was created, pushed, overwritten, or made public by this workflow. Existing remote visibility was not changed.

Restore network/DNS access for the authenticated command execution environment, then resume remote account/repository preflight. Hugging Face repositories must be created private first, uploaded, verified, and only then made public. No force-push or overwrite of unexpected remote contents is authorized.

## Local checks completed

- All six package checksums passed before documentation updates; refreshed checksums cover the final documentation.
- All 21 intended dataset files are present in both dataset packages and match the approved SHA-256 values byte-for-byte. Dataset payload: 67,810,370 bytes per repository.
- All four fine-tuned models have recorded successful offline SentenceTransformer loading and seven-input encoding tests: finite (7, 768) embeddings, correct E5 query/passage prefixes and unprefixed MPNet inputs.
- All 40 immutable model/tokenizer/config files retain their approved hashes. No models were retrained or changed.
- Checked credential/private-key patterns found no matches. No private directories, experimental Python source, or intermediate prediction outputs are included. Model weights are absent from GitHub.
- GitHub maximum file size is 14,588,112 bytes, below the 100 MiB per-file limit.
- Final GitHub, dataset, and four model identifiers have replaced repository placeholders throughout authored documentation. Dataset-card YAML, referenced local data paths, and existing scientific descriptions are preserved.
- Dataset/model card rendering and remote checksum comparisons remain unverified because no upload was possible.

## Local package inventories

Counts and bytes below exclude the root SHA256SUMS and this publication report to avoid self-referential sizes; they include the prepared payload and other documentation.

| Package | Files | Bytes |
|---|---:|---:|
| GitHub | 38 | 67893342 |
| HF dataset | 36 | 67894110 |
| Joint E5 | 17 | 438981719 |
| Joint MPNet | 17 | 438942055 |
| CWE-FT E5 | 17 | 438981547 |
| CWE-FT MPNet | 17 | 438941883 |

## Remaining metadata

The paper URL and full proceedings metadata have not been supplied. No new license for author-created materials or fine-tuned contributions has been selected; existing upstream attribution and terms remain preserved. No license permission or experimental result is invented.

## Resume criteria

1. Verify authenticated account identities and the existing GitHub repository/default branch without overwriting unexpected changes.
2. Upload the exact dataset and model packages; create Hugging Face repositories private initially.
3. Compare remote inventories, sizes, and file hashes/downloaded bytes against staged manifests; confirm card rendering and required model files.
4. Make only successfully verified Hugging Face repositories public.
5. Replace this blocked report with actual upload/verification results and commit the sanitized report to the existing GitHub repository.
