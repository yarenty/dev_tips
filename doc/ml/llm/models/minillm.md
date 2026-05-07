---
title: MiniLLM — knowledge distillation for LLMs (Microsoft Research)
main_link: https://github.com/microsoft/LMOps/tree/main/minillm
keywords: [minillm, knowledge-distillation, kd, microsoft-research, reverse-kl, lmops, small-llms]
status: reviewed
review_date: 2026/05/03
---

# MiniLLM — knowledge distillation for LLMs (Microsoft Research)

**Main link:** <https://github.com/microsoft/LMOps/tree/main/minillm>

## Summary

MiniLLM is a knowledge-distillation method (and reference implementation) from Microsoft Research's LMOps group, published at ICLR 2024 (Gu, Dong, Wei, Huang — *Knowledge Distillation of Large Language Models*). The headline contribution is replacing the standard forward-KL distillation objective — which encourages the student to cover all of the teacher's modes, including unlikely ones, leading to "mode-averaged" outputs — with a **reverse-KL** objective that focuses the student on the high-probability regions of the teacher distribution. The associated [Hugging Face MiniLLM org](https://huggingface.co/MiniLLM) hosts the distilled checkpoint releases. The technique is generic — applied to GPT-2, OPT, Llama-family bases — and is one of the foundational references for the modern wave of strong-small-model distillations (DeepSeek-R1-Distill, Llama 3.2-1B, Phi-3-mini, etc.).

## Insight

Reach for MiniLLM as the **canonical reference** when you're designing a distillation pipeline and want a baseline that's known to outperform vanilla teacher-forced KD. The reverse-KL trick shows up everywhere now: DeepSeek-R1's distillation of reasoning into Qwen and Llama students, many "small but capable" instruct models, on-policy distillation methods like GKD. The MiniLLM repo also pioneered a number of practical recipes — sample-from-student / teacher-mix corpora, length normalisation tricks for stable RL-style training — that were absorbed into later libraries (TRL, OpenRLHF, etc.).

That said, in 2025 you would rarely deploy MiniLLM-as-released; you'd take its **method** and apply it via a modern training framework. The repo's value is the paper + reference code, not as a productionised library. If you only want the *result* (a small strong model), pick from the modern distilled checkpoints — DeepSeek-R1-Distill-Qwen-1.5B/7B, Llama 3.2-1B/3B, Phi-3-mini-4k-instruct, Qwen2.5-0.5B/1.5B — most of which use techniques descended from this line of work.

## Similar / related topics

- DeepSeek-R1-Distill — the most influential modern distillation; Qwen/Llama students of R1. See [[deepseek]].
- Generalised Knowledge Distillation (GKD) — Google DeepMind 2023; on-policy KD that also uses reverse-KL-shaped objectives.
- DistilBERT / TinyBERT / MobileBERT — the older BERT-era distillation lineage.
- Hinton et al. 2015 — the original "Distilling the Knowledge in a Neural Network" paper (forward-KL).
- TRL / OpenRLHF / trlX — modern Hugging Face / community libraries where you'd implement MiniLLM-style training today.

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[deepseek]]
- [[llama]]
- [[../../finetuning/README|finetuning]]

## Keywords

`#minillm` `#knowledge-distillation` `#kd` `#microsoft-research` `#reverse-kl` `#lmops` `#small-llms`

## References / raw notes

### Canonical entry points

- Microsoft LMOps repo (MiniLLM subdir): <https://github.com/microsoft/LMOps/tree/main/minillm>
- Hugging Face org with checkpoints: <https://huggingface.co/MiniLLM>
- Paper: Gu, Dong, Wei, Huang — *Knowledge Distillation of Large Language Models* (ICLR 2024) — <https://arxiv.org/abs/2306.08543>

The HF org page hosts distillations of GPT-2, OPT-1.3B, and Llama-7B-class students; useful as a reproducible benchmark.
