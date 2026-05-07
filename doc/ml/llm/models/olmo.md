---
title: OLMo — Allen AI's fully open language model (weights + data + training code)
main_link: https://allenai.org/olmo
keywords: [olmo, allen-ai, ai2, fully-open, dolma, open-source-llm, pythia, oss]
status: reviewed
review_date: 2026/05/03
---

# OLMo — Allen AI's fully open language model (weights + data + training code)

**Main link:** <https://allenai.org/olmo>

## Summary

OLMo (Open Language Model) is the Allen Institute for AI's flagship "**fully open**" LLM family — meaning the weights, the training corpus (Dolma), the training code (OLMo-core), all intermediate checkpoints, the data-mix recipes, and the evaluation harness are all public. The lineage runs **OLMo 1** (Feb 2024, 1B / 7B, the initial release on the Dolma corpus) → **OLMo 1.7** (Apr 2024, recipe refinements) → **OLMo 2** (Nov 2024, 7B / 13B with substantially improved performance via OLMo-mix-1124 pretraining + Dolmino-mix-1124 mid-training) → **OLMo 2 1B** (the smallest member of the OLMo 2 family). The companion **Tülu** project provides the open instruction-tuning recipes layered on top.

## Insight

OLMo's importance is *political and scientific*, not commercial: it's the existence proof that you can ship a frontier-grade open-weights model with **everything** that produced it, allowing actually-reproducible science. Compare to Llama (weights only — you can't audit the data), DeepSeek (weights + papers — recipe is described but corpus is closed), Mistral (weights with apache licence but corpus closed). The Open Source Initiative's 2024 *Open Source AI Definition* debate was largely about whether weights-only counts as "open source"; OLMo represents the maximalist position that says no.

For everyday use OLMo 2 7B/13B are competitive with Llama 3.1 8B and Mistral-Nemo at the same size — fine if you want a "100% open" stack but not state of the art. The **real value** of OLMo is the surrounding artefacts: Dolma is the cleanest large-scale public pretraining corpus available, the intermediate checkpoints are gold for studying training dynamics (a research niche basically only Pythia previously occupied), and the OLMo-core training stack is probably the cleanest reference codebase for "how do you actually train an LLM end-to-end" outside megacorp labs. Pythia (EleutherAI, 2023) is the spiritual predecessor — same "release everything" ethos at a smaller scale.

## Similar / related topics

- Pythia (EleutherAI) — the earlier "fully open" reference family with intermediate checkpoints; still the canonical training-dynamics dataset.
- BLOOM / BLOOMZ (BigScience) — the multilingual fully-open precursor (176B); historically important, weaker by 2025 standards.
- LLM360 (Petuum / MBZUAI) — competing "fully open" effort (Amber, CrystalCoder, K2).
- DCLM-Pool / FineWeb — competing high-quality public pretraining corpora to Dolma.
- Tülu — Allen AI's open instruction-tuning recipes; usually paired with OLMo bases.

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[llama]]
- [[deepseek]]
- [[mplug]]
- [[../../finetuning/README|finetuning]] — Tülu post-training recipes.
- [[../../data/public|ml/data/public]] — public datasets including Dolma, FineWeb, RedPajama.

## Keywords

`#olmo` `#allen-ai` `#ai2` `#fully-open` `#dolma` `#open-source-llm` `#pythia` `#oss`

## References / raw notes

### Canonical entry points

- Project home (Allen AI): <https://allenai.org/olmo>
- GitHub: <https://github.com/allenai/OLMo>
- Allen AI on Hugging Face: <https://huggingface.co/allenai>

### OLMo 2 (current — Nov 2024 / Apr 2025)

> We introduce OLMo 2 1B, the smallest model in the OLMo 2 family. OLMo 2 was pre-trained on OLMo-mix-1124 and uses Dolmino-mix-1124 for mid-training. OLMo 2 is the latest in a series of Open Language Models designed to enable the science of language models. We have released all code, checkpoints, logs, and associated training details on GitHub.

- OLMo 2 1B: <https://huggingface.co/allenai/OLMo-2-0425-1B>
- OLMo 2 7B / 13B: see <https://huggingface.co/allenai>

### Pretraining data

- Dolma corpus: <https://huggingface.co/datasets/allenai/dolma>
- Dolmino-mix-1124 (mid-training data): <https://huggingface.co/datasets/allenai/dolmino-mix-1124>

### Companion projects

- **Tülu** (post-training / instruction-tuning recipes): <https://github.com/allenai/open-instruct>
- **Paloma** (perplexity-eval suite for OLMo training).
- **OLMo-core** (training framework that produced the OLMo 2 series).
