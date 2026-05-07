---
title: DeepSeek — open-weights MoE family (V2 / V3 / R1 / OCR)
main_link: https://www.deepseek.com/
keywords: [deepseek, deepseek-r1, deepseek-v3, deepseek-coder, moe, grpo, reasoning, open-weights, china]
status: reviewed
review_date: 2026/05/03
---

# DeepSeek — open-weights MoE family (V2 / V3 / R1 / OCR)

**Main link:** <https://www.deepseek.com/>

## Summary

DeepSeek (深度求索) is a Chinese AI lab affiliated with the High-Flyer quant fund that has shipped one of the most influential open-weights LLM lineages of 2024-2025: **DeepSeek-V2** (May 2024, 236B-total / 21B-active MoE with multi-head latent attention) → **DeepSeek-V3** (Dec 2024, 671B-total / 37B-active, the first OSS model to clearly compete with frontier closed models on most benchmarks) → **DeepSeek-R1** (Jan 2025, V3-base + GRPO RL, the first open reasoning model competitive with OpenAI o1) → distilled variants (R1-Distill-Qwen-1.5B/7B/14B/32B and R1-Distill-Llama-8B/70B). Side branches: **DeepSeek-Coder / Coder-V2**, **DeepSeek-Math**, **DeepSeek-VL2**, **DeepSeek-V3.1** (2025 update), and **DeepSeek-OCR** (a specialised OCR/document-understanding model). Hugging Face's [open-r1](https://github.com/huggingface/open-r1) project is the reference open reproduction of the R1 recipe.

## Insight

DeepSeek's importance is structural, not just rank-on-leaderboard: they showed that (a) a **true MoE architecture** (with auxiliary-loss-free load balancing and FP8 training) is now the obvious choice at the frontier, (b) **GRPO** (Group Relative Policy Optimisation, their PPO simplification) plus pure-RL post-training can produce reasoning behaviour without any SFT step, and (c) **distillation from a strong reasoner** to a small dense model (R1-Distill-Qwen-1.5B beats some 70B chat models on math) is a far cheaper way to get reasoning into edge sizes than retraining from scratch. They publish the recipe in actual papers, not just blog posts — so the rest of the field can copy them, which is most of the open-weights movement in 2025.

The practical picks: **DeepSeek-V3** is the default open-weights chat model where you can afford the MoE inference (vLLM, SGLang, official API); **R1** is the default open reasoning model; **R1-Distill-Qwen-7B/14B** is the sweet-spot for a single-GPU local reasoning model. The export-restricted compute angle is real — DeepSeek trained V3 on H800s, not H100s, and a lot of their efficiency tricks (FP8, MoE expert routing, MLA attention) come directly from that constraint. Don't ignore the licence: DeepSeek's weights are MIT-licensed (R1) or use a permissive DeepSeek Licence (V3); both are friendlier than Llama's community licence.

## Similar / related topics

- Qwen (Alibaba) — the other dominant Chinese open-weights line; Qwen2.5 / Qwen3 + Qwen-Coder + Qwen2.5-VL.
- Llama (Meta) — the OG OSS family; the comparison everyone is implicitly making. See [[llama]].
- Mistral / Mixtral — French MoE family; the original modern open-weights MoE.
- OLMo — Allen AI's "fully open" model (data + code + weights). See [[olmo]].
- OpenAI o1 / o3 — the closed-source reasoning lineage that R1 caught up to.

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[../runtimes/README|llm/runtimes]] — vLLM / SGLang / Ollama all support DeepSeek.
- [[../inspection/leaderboards|inspection/leaderboards]]
- [[../../finetuning/README|finetuning]] — open-r1 ships training recipes.
- [[llama]]
- [[olmo]]

## Keywords

`#deepseek` `#deepseek-r1` `#deepseek-v3` `#deepseek-coder` `#moe` `#grpo` `#reasoning` `#open-weights` `#china`

## References / raw notes

### Canonical entry points

- Project home: <https://www.deepseek.com/>
- DeepSeek on Hugging Face: <https://huggingface.co/deepseek-ai>
- DeepSeek on GitHub: <https://github.com/deepseek-ai>

### Key model releases

| Model | When | What's new |
|-------|------|------------|
| DeepSeek-V2 | May 2024 | 236B/21B-active MoE; introduced **MLA** (Multi-head Latent Attention) for KV-cache compression |
| DeepSeek-Coder-V2 | Jul 2024 | Coding specialisation built on V2 |
| DeepSeek-V3 | Dec 2024 | 671B/37B-active; FP8 training; auxiliary-loss-free load balancing; ~5.5M$ training cost |
| DeepSeek-R1 | Jan 2025 | V3-base + pure-RL via **GRPO**; reasoning competitive with o1; MIT-licensed weights |
| DeepSeek-R1-Distill-* | Jan 2025 | Reasoning distilled into Qwen 1.5/7/14/32B and Llama 8/70B |
| DeepSeek-V3.1 | 2025 | V3 update |
| DeepSeek-VL2 | 2025 | Vision-language MoE |
| DeepSeek-OCR | 2025 | Specialised OCR / doc-understanding model |

### open-r1 — reference open reproduction

<https://github.com/huggingface/open-r1>

Hugging Face's project to reproduce the DeepSeek-R1 recipe end-to-end on open data. Useful as a working example of GRPO + reward design + distillation pipelines.

Companion blog post: <https://huggingface.co/blog/open-r1>

Reasoning-RL diagram: <https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/open-r1/rl.png>

### Distilled checkpoints

The distilled R1 models punch well above their parameter count on reasoning benchmarks. Convenient picks:

- 1.5B (laptop-CPU friendly): <https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B>
- 14B (single-GPU sweet spot): <https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-14B>
- 70B (best distilled): <https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B>

### Talks / explainers

- <https://www.youtube.com/watch?v=kv8frWeKoeo> — early R1 walkthrough.
