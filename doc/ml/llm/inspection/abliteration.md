---
title: Abliteration — un-aligning LLMs by orthogonalising the refusal direction
main_link: https://huggingface.co/blog/mlabonne/abliteration
keywords: [abliteration, refusal-direction, alignment, safety-research, activation-steering, transformerlens]
status: reviewed
---

# Abliteration — un-aligning LLMs by orthogonalising the refusal direction

**Main link:** <https://huggingface.co/blog/mlabonne/abliteration>

## Summary

Abliteration is a technique, popularised by Maxime Labonne in mid-2024, for removing safety-tuned refusals from an instruction-tuned LLM **without further training**. It builds on the Anthropic-style "refusal direction" finding (Arditi et al., 2024) — that an aligned model represents "should I refuse?" as a single linear direction in residual-stream activation space — and **orthogonalises** the model's weights against that direction so the refusal signal can no longer be expressed. The Sumandora repo linked here is a minimal proof-of-concept that does the same thing without TransformerLens, so it works on most Hugging Face Transformers checkpoints.

## Insight

Abliteration is best understood as a **safety-research artefact**, not a practical un-alignment recipe. The technique is conceptually elegant — find one linear direction in activation space, project the weights onto its orthogonal complement, and the refusal behaviour collapses — but the practical value on production models is heavily debated:

- Quality often degrades sharply: an abliterated 70B model may answer questions it previously refused, but it also frequently produces nonsense, repetitive output, or loses instruction-following nuance on **unrelated** tasks (because the refusal direction isn't perfectly orthogonal to other useful features).
- It only works on the *behavioural* refusal layer; the model's underlying knowledge boundaries (what it learned vs didn't) are unchanged.
- Recovery via a small DPO / SFT pass is possible (and is what e.g. Huihui's "abliterated-then-fixed" Ollama tags do).
- For un-alignment that **preserves capability**, fine-tuning on jailbreak-shaped data or DPO-with-negative-rewards typically wins; for un-alignment that **just works at inference time**, activation-steering at runtime (without modifying weights) is more conservative.

The real research significance is what abliteration **demonstrates**: alignment in current RLHF/DPO-tuned models is shockingly fragile because so much of it lives in a low-dimensional subspace. That's a useful data point for safety researchers and a sobering one for anyone betting on RLHF as their safety story.

## Similar / related topics

- Arditi et al. 2024, *Refusal in LLMs is mediated by a single direction* — the underlying research finding.
- TransformerLens — Neel Nanda's library for mechanistic-interpretability hooks; the standard tool for activation-steering work.
- DPO with negative rewards / "uncensored" fine-tunes — train-time alternative (Eric Hartford's Dolphin series, Wizard-Vicuna-Uncensored).
- Activation steering / representation engineering — runtime alternative; modifies activations during inference, not weights.
- Anthropic's Sparse Autoencoders — the "scaling monosemanticity" line of work, finding interpretable features without orthogonalising them away.

## Internal links
- [[README|llm/inspection]]
- [[../README|llm]]
- [[../models/llama]] — common abliteration target.
- [[../runtimes/ollama|llm/runtimes/ollama]] — most published abliterated checkpoints ship as Ollama tags.
- [[../../finetuning/README|finetuning]] — alternative un-alignment via DPO/SFT.

## Keywords

`#abliteration` `#refusal-direction` `#alignment` `#safety-research` `#activation-steering` `#transformerlens`

## References / raw notes

### Canonical write-ups

- Maxime Labonne's blog post (the explainer most people read): <https://huggingface.co/blog/mlabonne/abliteration>
- Original research: Arditi, Obeso, et al., *Refusal in Language Models Is Mediated by a Single Direction*, 2024 — <https://arxiv.org/abs/2406.11717>

### Minimal implementation — Sumandora/remove-refusals-with-transformers

<https://github.com/Sumandora/remove-refusals-with-transformers>

A crude, proof-of-concept implementation that removes refusals from an LLM **without** using TransformerLens. Works on most HF-Transformers-compatible models; tested on an RTX 2060 6GB (mostly <3B models, but the recipe scales).

Caveats from the repo:

- Some models with custom implementations break the assumption that layers live at `model.model.layers` — e.g. some Qwen variants put them at `model.transformer.h`.
- Quantisation can be mixed between the direction-computation and inference passes.

Workflow:

1. Set the target model + quantisation in `compute_refusal_dir.py`.
2. Run it to extract the refusal direction (and a small "harmless" / "harmful" prompt set is used to identify the direction).
3. Run `inference.py` with the direction subtracted from the weights to chat with the abliterated model.

### Pre-baked Ollama tags

<https://ollama.com/huihui_ai/qwen3-abliterated> — Huihui_AI's published abliterated tags. Useful as a "what does this actually look like in practice" sandbox. Ollama-side toggle: `/no_think` after the prompt to disable Qwen3's thinking mode.
