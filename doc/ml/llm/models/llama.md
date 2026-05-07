---
title: Llama — Meta's open-weights LLM family (Llama 1/2/3/4)
main_link: https://www.llama.com/
keywords: [llama, meta, open-weights, llm, llama-2, llama-3, llama-4, code-llama, llama-stack]
status: reviewed
---

# Llama — Meta's open-weights LLM family (Llama 1/2/3/4)

**Main link:** <https://www.llama.com/>

## Summary

Llama is Meta AI's family of decoder-only transformer LLMs released with downloadable weights and a permissive-ish community licence. The lineage runs Llama 1 (Feb 2023, originally research-only, leaked) → Llama 2 (Jul 2023, first OSS-with-licence, 7B/13B/70B + Chat) → Code Llama (Aug 2023) → Llama 3 (Apr 2024, 8B/70B) → Llama 3.1 (Jul 2024, added 405B + 128k context) → Llama 3.2 (Sep 2024, vision + edge sizes 1B/3B) → Llama 3.3 (Dec 2024, 70B-that-matches-405B) → Llama 4 (Apr 2025: Maverick 17B-active MoE, Scout 109B-MoE, Behemoth 2T preview). Around the models Meta also ships **Llama Stack** (a unified API/distribution layer) and the **Llama Cookbook** (recipes for inference / fine-tuning / agentic use).

## Insight

Llama is the load-bearing pillar of the open-weights LLM ecosystem: most fine-tunes, distillations, instruction-tuned derivatives, and runtime benchmarks (`llama.cpp`, vLLM, Ollama, MLX) trace back to a Llama base. If you fine-tune anything yourself, Llama 3.x 8B is still the default starting point for experiments; Llama 3.3 70B is the practical "good open-weights chat model" sweet spot; Llama 4 Scout/Maverick brought MoE to the family but adoption has been slower than the 3.x series, partly because the licence still requires acceptance and the MoE inference story is more complex.

Watch out for licence drift: "Llama Community License" is **not** OSI-open — it has an MAU clause (originally 700M) blocking the largest cloud providers from running it, and an acceptable-use policy. Don't conflate **Llama** (the model, by Meta) with **`llama.cpp`** (the C/C++ inference runtime by Georgi Gerganov, lives in `[[../runtimes/README|llm/runtimes]]`); they are independent projects, the runtime just happens to support the model and many others. Code Llama and Llama Guard are now legacy — Llama 3.x base models do code competently, and Llama Guard has been superseded by Llama Guard 3 / Prompt Guard.

## Similar / related topics

- DeepSeek — Chinese open-weights MoE family; the 2024-2025 challenger. See [[deepseek]].
- Mistral / Mixtral — Mistral AI's open-weights line (Mistral 7B, Mixtral 8x7B/8x22B MoE, Codestral).
- Qwen — Alibaba's open-weights family (Qwen2.5, Qwen3, Qwen-VL, Qwen-Coder); often beats Llama at the same size.
- Gemma — Google's open-weights cousin to Gemini (Gemma 2, Gemma 3 multimodal).
- OLMo — fully-open (weights + data + training code) alternative from Allen AI. See [[olmo]].

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[../runtimes/README|llm/runtimes]] — `llama.cpp`, Ollama, vLLM, etc.
- [[deepseek]]
- [[olmo]]
- [[../../finetuning/README|finetuning]]
- [[../../agents/README|agents]]

## Keywords

`#llama` `#meta` `#open-weights` `#llm` `#llama-2` `#llama-3` `#llama-4` `#code-llama` `#llama-stack`

## References / raw notes

### Canonical entry points

- Project home: <https://www.llama.com/>
- Meta-Llama org on GitHub: <https://github.com/meta-llama>
- Llama models on Hugging Face: <https://huggingface.co/meta-llama>

### Llama Stack — unified API across the ecosystem

<https://github.com/meta-llama/llama-stack>

Llama Stack defines and standardises the core building blocks for AI applications around the Llama family. It provides:

- A unified API layer for **Inference, RAG, Agents, Tools, Safety, Evals, and Telemetry**.
- A plugin architecture so the same APIs can be backed by different providers (local dev, on-prem, cloud, mobile).
- Pre-packaged "distributions" for one-stop deployment.
- SDKs in Python, TypeScript, iOS, and Android.

Example apps using the stack: <https://github.com/meta-llama/llama-stack-apps> — multi-step reasoning, tool use (built-in and zero-shot), and Llama-Guard-based safety.

### Llama Cookbook — recipes

<https://github.com/meta-llama/llama-cookbook>

Official recipes for inference, fine-tuning, and end-to-end use cases on Llama text and vision models.

### llama-agentic-system

<https://github.com/meta-llama/llama-agentic-system> — earlier agentic-loop reference implementation; mostly superseded by Llama Stack's Agents API.

### Code Llama (legacy)

<https://github.com/meta-llama/codellama>

Family of code-specialised models (7B / 13B / 34B) fine-tuned from Llama 2 on code-heavy data, with Python and Instruct flavours, 16k-token training context, and infilling support on the 7B/13B variants. Largely superseded by general-purpose Llama 3.x and by competitors (DeepSeek-Coder, Qwen-Coder, Codestral, StarCoder2) — see [[jetbrains_mellum]] for one current FIM-focused competitor.

### Llama 2 / Llama 3 deprecated repos

- <https://github.com/meta-llama/llama> — Llama 2 reference inference code.
- <https://github.com/meta-llama/llama3> — Llama 3 reference inference code.

Both are now maintenance-mode; for new work use Hugging Face Transformers + the canonical HF repos under `meta-llama/`.

### llama.cpp — inference runtime (different project)

<https://github.com/ggml-org/llama.cpp> — Georgi Gerganov's C/C++ inference of Llama (and many other) models. Canonical home for GGUF quantisation; the engine inside Ollama, LM Studio, etc. Detailed in [[../runtimes/README|llm/runtimes]].
