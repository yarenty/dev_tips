---
title: Inspectus — attention and activation visualisation in Jupyter
main_link: https://github.com/labmlai/inspectus
keywords: [inspectus, attention-visualisation, interpretability, jupyter, labml, debugging]
status: reviewed
review_date: 2026/05/03
---

# Inspectus — attention and activation visualisation in Jupyter

**Main link:** <https://github.com/labmlai/inspectus>

## Summary

Inspectus is a small, install-and-go visualisation library from [labml.ai](https://labml.ai/) for poking at LLM internals from a Jupyter notebook. It renders attention matrices, attention-head views, and activation distributions over a tokenised string with a single Python call, accepting attention tensors from any HF-Transformers-compatible model. The headline use case is "I have a confusing model output — what was the model attending to at each position?" without having to wire up TransformerLens or write your own matplotlib.

## Insight

Reach for Inspectus when you want **fast, casual** attention/activation introspection inside a notebook — debugging a prompt, sanity-checking a fine-tune, sharing a visual with a teammate. It's deliberately lighter-weight than TransformerLens (which is the right tool for serious mechanistic-interpretability research where you need hooks, ablations, and patching) and prettier than rolling your own attention-heatmap with seaborn.

For most real interpretability work in 2024-2025 the heavier-weight stack wins: **TransformerLens** for activation patching and circuit analysis, **BertViz** as the historical baseline visualiser still worth knowing, **NN-CIRCUITS / SAEViz** for sparse-autoencoder feature visualisation (Anthropic's "scaling monosemanticity" line), and **OpenAI's `transformer-debugger`** if you specifically need GPT-2 internals. Inspectus's niche is the moment between "raw attention tensor" and "I want to share a visual on Twitter" — fast iteration, low ceremony.

## Similar / related topics

- BertViz (jessevig) — the original attention visualiser; head/model/neuron views; older but still pedagogically clearest.
- TransformerLens (Neel Nanda) — the standard mechanistic-interpretability library; hooks, ablation, activation patching.
- circuitsvis — the React component library underneath many TL-based visualisations.
- SAEViz / Anthropic's interpretability tooling — for sparse-autoencoder-feature-level inspection.
- `transformer-debugger` (OpenAI) — GPT-2-specific interactive debugger.

## Internal links
- [[README|llm/inspection]]
- [[../README|llm]]
- [[../../tools/README|ml/tools]]
- [[../../knowledge_graph/papers|kg/papers]] — interpretability papers often live alongside KG/embedding work.

## Keywords

`#inspectus` `#attention-visualisation` `#interpretability` `#jupyter` `#labml` `#debugging`

## References / raw notes

> Inspectus is a versatile visualisation tool for large language models. It runs smoothly in Jupyter notebooks via an easy-to-use Python API. Inspectus provides multiple views, offering diverse insights into language model behaviours.

- Repo + docs: <https://github.com/labmlai/inspectus>
- labml.ai: <https://labml.ai/> — the same group also publishes [`labml-nn`](https://nn.labml.ai/), an annotated-implementations site that's an excellent companion read.
