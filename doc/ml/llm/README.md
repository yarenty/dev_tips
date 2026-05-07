---
title: LLM — models, runtimes, prompting, inspection, OS-shaped agents
keywords: [llm, models, runtimes, system-prompts, tokenizers, inspection, llm-os]
status: reviewed
review_date: 2026/05/03
---

# LLM — models, runtimes, prompting, inspection, OS-shaped agents

The LLM hub. This section gathers model-family notes, ways to actually run LLMs (local + hosted), the system-prompt corpus, the tokenization layer, evaluation/inspection tooling, and the higher-order architectural framings (LLM OS, agentic loops). For closely-related sections see the cross-section see-also at the bottom.

## Subsections

- [Models](models/README.md) — open-weights model families: [Llama](models/llama.md), [DeepSeek](models/deepseek.md), [OLMo](models/olmo.md), [mPLUG](models/mplug.md), [JetBrains Mellum](models/jetbrains_mellum.md), [MiniLLM](models/minillm.md), [Strawberry browser](models/strawberry.md).
- [Runtimes](runtimes/README.md) — how to run LLMs: Ollama (+ Ubuntu LAN sharing), `llama.cpp`, vLLM, Goose, Rig, rsllm, providers, extensions.
- [System prompts](system_prompts/README.md) — the leaked-prompts corpus and prompting techniques.
- [Inspection](inspection/README.md) — leaderboards (Chatbot Arena, SEAL), interpretability (Inspectus), alignment surgery (abliteration).

## Top-level articles

- [Tokenizers — the silent layer that shapes LLM behaviour](tokenizers.md) — BPE / WordPiece / Unigram / SentencePiece / tiktoken; the strawberry-counts-its-Rs gotcha; tokenizer-free trends.
- [LLM OS — Karpathy's "LLM as kernel" framing](llm_os.md) — the architectural framing for modern agentic systems.

## Decision shortcut

| You want… | Reach for |
|-----------|-----------|
| The default open-weights chat model in 2025 | Llama 3.3 70B or Qwen 2.5/3 — see [[models/llama]] |
| Best open-weights reasoning | DeepSeek-R1 or distilled variants — see [[models/deepseek]] |
| A *truly* open (data + code + weights) model | OLMo 2 — see [[models/olmo]] |
| Run a model locally with one command | Ollama — see [[runtimes/ollama|runtimes/ollama]] |
| A high-throughput inference server | vLLM / SGLang — see [[runtimes/README|runtimes]] |
| Compare models objectively | [[inspection/leaderboards|leaderboards]] + [[inspection/seal|SEAL]] |
| Visualise attention/activation in a notebook | [[inspection/inspectus|Inspectus]] |
| Understand the architectural framing | [[llm_os]] |
| Understand why "strawberry" has 3 R's confuses LLMs | [[tokenizers]] |
| Adapt a model | [[../finetuning/README|finetuning]] |
| Build an agent on top | [[../agents/README|agents]] + [[../mcp/README|mcp]] |

## Cross-section see-also

- [[../README|ml]] — parent ML landing.
- [[../rag/README|rag]] — retrieval-augmented generation.
- [[../agents/README|agents]] — agent frameworks and patterns.
- [[../mcp/README|mcp]] — Model Context Protocol (the syscall ABI for LLMs).
- [[../finetuning/README|finetuning]] — LoRA, DPO, GRPO, distillation.
- [[../tools/README|ml/tools]] — Candle, MLX, transformers, etc.
- [[../memory/README|memory]] — agent memory architectures.
- [[../frameworks/README|ml/frameworks]] — DSPy, LangGraph, agno/phidata.

## Keywords

`#llm` `#models` `#runtimes` `#system-prompts` `#tokenizers` `#inspection` `#llm-os`
