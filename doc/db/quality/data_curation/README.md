---
title: Data curation
keywords: [data-curation, ml, training-data, annotation, synthetic-data, active-learning, deduplication]
status: reviewed
review_date: 2026/05/03
---

# Data curation

*Data curation* is the deeper, AI-shaped cousin of data quality. Where
classical data quality asks "is this row valid against the schema?", curation
asks "is this dataset *the right one* to learn from?" — which involves active
selection, annotation, deduplication, synthetic generation, and balancing.

The work typically includes:

- **Selection / filtering** — drop low-quality, off-topic, near-duplicate, or
  toxic samples. Tools: MinHash dedup, model-based quality classifiers,
  perplexity filters, fastText language ID.
- **Annotation** — label or rate samples (RLHF, preference pairs, NER spans,
  bounding boxes). Tools: Label Studio, Argilla, Prodigy, Snorkel for weak
  supervision.
- **Synthetic / augmented data** — generate examples to fill gaps; common in
  fine-tuning and instruction tuning.
- **Active learning** — pick the *next* most valuable samples to label given
  a model's current uncertainty.
- **Deduplication and decontamination** — especially against benchmarks, to
  avoid train/test leakage.

## Why a separate concept

Curation cares about distributional properties (coverage, balance, novelty)
that schema-shaped validation can't see. A dataset can be 100% valid by every
Great Expectations rule and still be useless because every row is a near
duplicate of every other.

## See also

- [[db/quality/README|quality]] — section landing.
- [[data_quality]] — the validation-shaped sibling.
- [[db/quality/data_quality_problems_in_AI/README|data quality in AI]] —
  failure modes specific to ML training.

## External pointers

- **Argilla** — open-source annotation / curation for LLM datasets.
- **Label Studio** — general-purpose annotation tool.
- **Snorkel** — programmatic / weak supervision.
- **NeMo Curator** (NVIDIA) — large-scale curation pipelines for LLM training.
- **Datatrove** (HuggingFace) — pipeline framework for processing massive
  text corpora.

## Keywords

`#data-curation` `#ml` `#training-data` `#annotation` `#synthetic-data` `#active-learning` `#deduplication`
