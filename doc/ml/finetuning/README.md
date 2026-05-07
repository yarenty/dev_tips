---
title: Fine-tuning — adapting LLMs to your data
keywords: [fine-tuning, lora, qlora, peft, rlhf, dpo, grpo, sft]
status: reviewed
review_date: 2026/05/03
---

# Fine-tuning — adapting LLMs to your data

> Continuing the training of a pretrained base model on task-specific data.
> When prompting and RAG aren't enough.

## What is fine-tuning?

Take a pretrained LLM (Llama 3, Qwen, Mistral, …) and train further on a curated dataset
to **shift its behaviour** — domain-specific style, instruction-following, tool-use,
preferred formats, safety policies, or new factual knowledge. In 2025 this almost never
means *full* fine-tuning of all weights; it means **PEFT** (parameter-efficient
fine-tuning) of 0.1–1% of weights via LoRA / QLoRA / DoRA / ReFT, on a single GPU.

## The decision tree (cost / effort, ascending)

1. **Prompting** — system prompt + few-shot examples. Cheapest, fastest, often enough.
2. **RAG** — retrieve relevant context at query time. The right answer when the issue is
   **knowledge gaps** rather than **behaviour gaps**.
3. **Fine-tuning** — the right answer when the model **already knows** the facts but you
   need it to **act differently** (style, format, tool-use, refusals, persona).
4. **Continued pretraining** — domain-adaptive pretraining on raw corpora; rare; needs
   millions of tokens.
5. **From scratch** — almost never the right answer outside frontier labs.

## Modern PEFT landscape

- **LoRA** ([Hu et al. 2021](https://arxiv.org/abs/2106.09685)) — low-rank update
  matrices `BA` added to frozen weights. The default. Rank 8–64 covers most cases.
- **QLoRA** ([Dettmers et al. 2023](https://arxiv.org/abs/2305.14314)) — LoRA on top of
  4-bit NF4 quantised weights. Lets you fine-tune 70B on a single A100/H100.
- **DoRA** ([Liu et al. 2024](https://arxiv.org/abs/2402.09353)) — decomposes LoRA into
  magnitude+direction; small accuracy bump, modest cost.
- **ReFT** ([Wu et al. 2024](https://arxiv.org/abs/2404.03592)) — intervenes on
  representations rather than weights; even fewer params than LoRA.
- **Galore / GaLore-Lite** — full-rank update with gradient compression; alternative
  philosophy to PEFT.

## Beyond SFT — alignment & preference methods

- **SFT** (supervised fine-tuning) — the standard `(prompt → completion)` cross-entropy.
- **RLHF** ([Christiano 2017](https://arxiv.org/abs/1706.03741)) — reward model + PPO.
  The OpenAI / Anthropic original; complex; mostly being replaced.
- **DPO** ([Rafailov 2023](https://arxiv.org/abs/2305.18290)) — Direct Preference
  Optimisation; no separate reward model; today's default for `(chosen, rejected)` data.
- **GRPO** (DeepSeek 2024, R1 paper) — Group-Relative PPO; the headline RL recipe behind
  the reasoning-model wave; surprisingly tractable at small scale.
- **KTO** ([Ethayarajh 2024](https://arxiv.org/abs/2402.01306)) — only needs `(prompt,
  completion, good/bad)` flags rather than pairs.
- **IPO** / **ORPO** / **SimPO** / **CPO** — DPO-family variants tuning the loss shape.

## Key libraries

| Library | Niche |
|---|---|
| HuggingFace [`transformers`](https://github.com/huggingface/transformers) + [`peft`](https://github.com/huggingface/peft) + [`trl`](https://github.com/huggingface/trl) | The substrate; PPO/DPO/ORPO/GRPO/KTO trainers |
| [Unsloth](unsloth.md) | 2× faster, 50–80% less VRAM via Triton kernels; single-GPU king |
| [Axolotl](https://github.com/axolotl-ai-cloud/axolotl) | YAML-config-driven recipes; multi-GPU |
| [torchtune](https://github.com/pytorch/torchtune) | PyTorch-native, opinionated, multi-GPU |
| [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory) | GUI + breadth; popular in CN |
| [bitsandbytes](https://github.com/bitsandbytes-foundation/bitsandbytes) | 4-bit / 8-bit quantisation backbone |
| [DeepSpeed](https://github.com/deepspeedai/DeepSpeed) / [FSDP](https://pytorch.org/docs/stable/fsdp.html) | Multi-GPU sharding for big models |
| [mistral-finetune](https://github.com/mistralai/mistral-finetune) | Single-vendor LoRA |
| [MLX-LM](https://github.com/ml-explore/mlx-lm) | Apple-Silicon-native fine-tuning |

## Articles in this section

- [[unsloth|Unsloth — 2× faster LoRA/QLoRA]] — the headline single-GPU library.
- [[fedsb|Fed-SB — Federated Silver Bullet]] — federated LoRA; communication-cost
  independent of client count.

## See also

- [[../llm/README|llm]] — LLM hub: models, runtimes, prompting.
- [[../llm/models/llama|llama]] — canonical fine-tuneable base.
- [[../llm/models/deepseek|deepseek]] — GRPO innovator; R1 reasoning recipe.
- [[../mcp/README|mcp]] — sibling section; tools for fine-tuned agents.
- [[../agents/README|agents]] — what fine-tuned models often power.
- [[../tools/README|tools]] — broader ML tooling.
- [[../frameworks/README|frameworks]] — agent / orchestration frameworks (LangGraph, DSPy).
- [[../memory/README|memory]] — runtime alternative to fine-tuning behaviour into weights.

## Keywords

`#fine-tuning` `#lora` `#qlora` `#peft` `#rlhf` `#dpo` `#grpo` `#sft`
