---
title: Zero-shot learning
main_link: https://arxiv.org/abs/2103.00020
keywords: [zero-shot, few-shot, in-context-learning, clip, llm, foundation-models]
status: reviewed
---

# Zero-shot learning

**Main link:** <https://arxiv.org/abs/2103.00020>

## Summary

Zero-shot learning is the task of classifying or acting on inputs from classes the model has **never
seen labelled examples of**. The term was coined in the computer-vision community (Lampert et al.
2009) and was historically built around hand-engineered class-attribute embeddings. The modern era
opened with CLIP (Radford et al. 2021, OpenAI) — joint image+text contrastive pretraining lets you
classify any image against an arbitrary list of natural-language class names, no labelled training
data required. In NLP, the GPT-3 paper (Brown et al. 2020) re-popularised the term for instruction-
followable LLMs that can perform unseen tasks from a prompt alone.

## Insight

Three things to keep clearly separated when reading the literature:

- **Zero-shot** — the task description (class names, instruction, schema) is provided at inference,
  but **no examples** are given. CLIP's "this is a photo of a {label}" prompt is the canonical case.
- **Few-shot / in-context learning** — a handful of examples (typically 1-32) are included in the
  prompt at inference time. The model's weights don't change. This is what GPT-3 made famous.
- **Few-shot fine-tuning** — actually update weights on the small example set. Sibling, not the same
  thing.

The practical 2025 takeaway: zero-shot via CLIP-style models or modern LLMs is genuinely good
enough for prototyping, exploration, and long-tail classes where you'd never collect labels — but
it's almost always **worse than a small fine-tuned classifier** on a fixed taxonomy with even
modest training data. Reach for zero-shot when (a) the label set is open or changes constantly,
(b) you're prototyping and don't yet have labels, or (c) you need to handle classes that were
genuinely unforeseen at training time. Don't reach for it when accuracy is critical and you can
afford to label a few hundred examples.

## Similar / related topics

- **Few-shot learning** — same family; with examples instead of without.
- **In-context learning** (ICL) — the LLM-era term for few-shot via prompt.
- **CLIP / open-vocabulary models** — the canonical vision zero-shot toolkit; SigLIP, OpenCLIP, ALIGN are siblings.
- **Prompt engineering** — the practical craft of getting good zero-shot behaviour out of an LLM.
- **Retrieval-augmented generation (RAG)** — adjacent: rather than zero-shot from priors, retrieve relevant context and condition on it.

## Internal links
<!-- reviewed -->
- [[../llm/README|llm]] — the LLM section where modern zero-shot lives.
- [[../rag/README|rag]] — sibling: when zero-shot isn't enough, retrieve context.
- [[../frameworks/README|frameworks]] — frameworks that ship CLIP / SigLIP / similar models.
- [[no_deep_learning_needed]] — counterpoint: classical models when you actually have labels.
- [[classification]] — sibling on labelled classification.
- [[pattern_learning]] — sibling parent topic.

## Keywords

`#zero-shot` `#few-shot` `#in-context-learning` `#clip` `#llm` `#foundation-models`

## References / raw notes

- The term-defining paper: [Lampert, Nickisch & Harmeling 2009, *Learning to detect unseen object classes by between-class attribute transfer*](https://ieeexplore.ieee.org/document/5206594) (CVPR'09).
- Modern vision: [Radford et al. 2021, *Learning Transferable Visual Models From Natural Language Supervision* (CLIP)](https://arxiv.org/abs/2103.00020), [SigLIP](https://arxiv.org/abs/2303.15343), [OpenCLIP](https://github.com/mlfoundations/open_clip).
- Modern NLP: [Brown et al. 2020, *Language Models are Few-Shot Learners* (GPT-3)](https://arxiv.org/abs/2005.14165).
- Survey: [Wang et al. 2019, *A Survey of Zero-Shot Learning*](https://dl.acm.org/doi/10.1145/3293318).
- Practical: HuggingFace [`zero-shot-classification`](https://huggingface.co/tasks/zero-shot-classification) and [`zero-shot-image-classification`](https://huggingface.co/tasks/zero-shot-image-classification) task pages.
