---
title: LLM models — open-weights and notable model families
keywords: [models, llm, open-weights, llama, deepseek, olmo, mplug]
status: reviewed
---

# LLM models — open-weights and notable model families

Curated notes on specific LLM model families that have crossed my radar — mostly open-weights work where the runtime / fine-tuning / inspection story is interesting. The general picks for production work (GPT-*, Claude, Gemini) sit one level up at [[../README|llm]] alongside the runtime / system-prompt sections.

## Frontier open-weights families

- [Llama — Meta's Llama 1/2/3/4 family](llama.md) — the load-bearing OSS LLM lineage; consolidates licence, lineage, Llama Stack, Code Llama, and the deprecated repos.
- [DeepSeek — V3 / R1 / OCR / specialised variants](deepseek.md) — Chinese MoE family that ate the 2025 open-weights conversation; includes the open-r1 reproduction story.
- [OLMo — Allen AI's *fully* open language model](olmo.md) — weights + data + training code + intermediate checkpoints; the gold-standard "open" reference vs Llama's "open weights only".

## Specialised / niche models

- [JetBrains Mellum-4B-base — coding FIM model](jetbrains_mellum.md) — JetBrains' fill-in-the-middle code model; powers their AI Assistant / Junie agent.
- [mPLUG — Alibaba's vision-language family](mplug.md) — DAMO Academy / AliceMind's mPLUG / mPLUG-2 / mPLUG-Owl / DocOwl line.
- [MiniLLM — Microsoft's KD-trained small LLMs](minillm.md) — knowledge-distillation reference (Microsoft Research).
- [Strawberry browser — AI-driven browser automation](strawberry.md) — *not* the OpenAI o1 codename; this is the "self-driving browser" product.

## Decision shortcut

| You want… | Reach for |
|-----------|-----------|
| The default open-weights chat model in 2025 | Llama 3.3 70B or Qwen2.5/Qwen3 (similar size) |
| Best open-weights reasoning | DeepSeek-R1 or its distilled siblings |
| A *truly* open (data + code + weights) model | OLMo 2 |
| A coding fill-in-the-middle model | DeepSeek-Coder-V2, Codestral, Qwen2.5-Coder, or [JetBrains Mellum](jetbrains_mellum.md) |
| Vision-language for documents | Qwen2.5-VL, [mPLUG-DocOwl](mplug.md), or LLaVA-NeXT |
| A small distilled model for edge/CPU | DeepSeek-R1-Distill-Qwen-1.5B/7B, Llama 3.2 1B/3B, [MiniLLM](minillm.md)-style distillations |

## Cross-section see-also

- [[../README|llm]] — parent landing.
- [[../runtimes/README|llm/runtimes]] — how to actually run these (Ollama, llama.cpp, vLLM, etc.).
- [[../inspection/README|llm/inspection]] — leaderboards / interpretability for the same models.
- [[../../finetuning/README|finetuning]] — for adapting them.
- [[../../tools/README|ml/tools]] — Candle, MLX, etc.

## Keywords

`#models` `#llm` `#open-weights` `#llama` `#deepseek` `#olmo` `#mplug`
