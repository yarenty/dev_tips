---
title: LLM providers — Goose's matrix and the broader landscape
main_link: https://block.github.io/goose/docs/getting-started/providers
keywords: [providers, openai, anthropic, ollama, bedrock, openrouter, litellm, groq]
status: reviewed
---

# LLM providers — Goose's matrix and the broader landscape

**Main link:** <https://block.github.io/goose/docs/getting-started/providers>

## Summary

Modern LLM clients (Goose, LangChain, LiteLLM, OpenRouter, etc.) all
have to wrestle with the same problem: there are now ~20 production
LLM endpoints with different APIs, auth shapes, rate limits, pricing,
and tool-calling support. This article catalogues the landscape from
two angles — Goose's officially supported provider list (Ollama,
Anthropic, OpenAI, Google, AWS Bedrock, Azure OpenAI, Databricks,
OpenRouter, …) and the broader 2024-2025 cloud-LLM market that any
runtime has to reason about.

## Insight

Three useful lenses on the provider landscape:

- **Frontier model vendors** — OpenAI (GPT-4o/5/o-series), Anthropic
  (Claude Sonnet/Opus/Haiku), Google (Gemini 2.x), DeepSeek
  (V3/R1), Mistral La Plateforme, xAI (Grok). You pick these for
  capability; price and rate limits vary wildly.
- **Inference clouds** — Together AI, Fireworks, Groq (LPU,
  ridiculously fast on Llama/Mixtral), Cerebras (CS-3, comparable
  story), Lambda, RunPod. They host open-weights models so you
  don't have to. Latency/throughput/cost vary; Groq+Cerebras win
  on latency, Together/Fireworks on model breadth.
- **Aggregators / proxies** — OpenRouter (one API key, hundreds of
  models, pay-as-you-go), [LiteLLM](https://github.com/BerriAI/litellm)
  (self-hosted multi-provider proxy with cost tracking), Portkey
  (commercial gateway). These are how you avoid hard-coding any
  single vendor.

For Goose specifically: the supported list is in the docs above; the
common starter combo is **Ollama for local + Anthropic or OpenAI for
the heavy lifting**. The DeepSeek-R1 path is appealing for reasoning
on-prem, but the native model doesn't tool-call — use the bundled
`michaelneale/deepseek-r1-goose` variant instead.

Cross-cutting concerns when picking a provider:

- **Tool/function calling support and quality** — varies by model.
- **Structured outputs / JSON mode** — OpenAI and Anthropic have it;
  some providers don't.
- **Context window** — 200k (Claude), 1M+ (Gemini 2.x, GPT-4.1),
  128k typical for open-weights.
- **Data residency / training opt-out** — important for enterprise.
- **Rate limits and burstiness** — the silent killer of agent loops.

## Similar / related topics

- OpenRouter — single-API aggregator across hundreds of models.
- LiteLLM — self-hosted multi-provider proxy with budgets/observability.
- Portkey — commercial AI gateway (caching, fallbacks, governance).
- Vercel AI SDK — TypeScript multi-provider client.
- LangChain `chat_models` — Python multi-provider abstraction.

## Internal links
<!-- reviewed -->
- [[goose]] — the agent runtime that consumes this provider list.
- [[ollama]] — the most common local provider in the Goose stack.
- [[extensions]] — sister concept; tool plugin shape.
- [[rig]] — Rust-side multi-provider client (the `rig` crate's `providers/` modules).
- [[../models/deepseek|models/deepseek]] — DeepSeek-R1 (incl. the Goose tool-calling fork).
- [[../models/llama|models/llama]] — the open-weights default.
- [[../README|llm]] — parent LLM hub.

## Keywords

`#providers` `#openai` `#anthropic` `#ollama` `#bedrock` `#openrouter` `#litellm` `#groq`

## References / raw notes

### Goose's supported providers

- <https://block.github.io/goose/docs/getting-started/providers>

### Notable entry: Ollama + DeepSeek-R1 for Goose

> Ollama — local model runner supporting Qwen, Llama, DeepSeek, and
> other open-source models. Because this provider runs locally, you
> must first download and run a model.
>
> Note: the native DeepSeek-R1 model does not support tool calling.
> A custom model is provided for use with Goose. The full version is
> 70B and requires a powerful device.

```sh
# install Ollama from https://ollama.com
ollama run michaelneale/deepseek-r1-goose
```

See [[ollama]] for the canonical Ollama notes and [[../models/deepseek|models/deepseek]]
for the DeepSeek family.
