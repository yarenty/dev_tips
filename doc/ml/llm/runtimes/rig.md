---
title: rig — Rust crate for building LLM applications
main_link: https://github.com/0xPlaygrounds/rig
keywords: [rig, rust, llm, agents, rag, embeddings, multi-provider]
status: reviewed
review_date: 2026/05/03
---

# rig — Rust crate for building LLM applications

**Main link:** <https://github.com/0xPlaygrounds/rig>

## Summary

`rig` is a Rust library by 0xPlaygrounds for building scalable,
modular, and ergonomic LLM-powered applications: a multi-provider
client (OpenAI, Anthropic, Cohere, Gemini, Ollama, xAI, Mistral,
Groq, Together, …), a typed tool/function-calling layer, agent and
pipeline abstractions, and embedding/vector-store integrations
(LanceDB, Qdrant, MongoDB Atlas, Milvus). Think of it as a Rust
answer to LangChain rather than to OpenAI's Python SDK.

## Insight

Rig occupies a real gap in the Rust ecosystem. Where you'd reach
for it, vs the alternatives:

| Want                                              | Reach for                       |
|---------------------------------------------------|---------------------------------|
| Bare OpenAI/Anthropic API calls in Rust           | `async-openai`, `anthropic-sdk-rs` |
| Single-vendor Ollama client                       | [[ollama]] + `ollama-rs`        |
| LangChain-shaped agents/RAG/tools in Rust         | **`rig`**                       |
| Earlier LangChain-port attempt                    | `llm-chain` (less actively maintained) |
| Run open-weights models *in process* (no server)  | `mistral.rs`, `kalosm`, `candle` |

Three things to know in practice:

- **Provider modules are first-class.** Each provider lives in
  `rig::providers::*` with its own auth/transport but a shared
  `CompletionModel`/`EmbeddingModel` trait. Swap models without
  rewriting agent code — same idea as LiteLLM/OpenRouter, but
  in-process.
- **`Agent` + tools is the typed sweet spot.** Tools are normal
  Rust functions exposed via the `tool` macro; types flow through
  with `serde`. This is much nicer than the dynamic dictionaries
  in Python LangChain.
- **It's Rust-LangChain, not Rust-LangGraph.** For long-lived,
  cyclic, checkpointed agents you'll outgrow rig's pipeline shape;
  there isn't yet a true LangGraph equivalent in Rust (you build
  it yourself on top).

The 0xPlaygrounds team is web3-adjacent (the `rig` README features
on-chain agents prominently), but the library itself is general-purpose.

## Similar / related topics

- LangChain / LangGraph (Python) — the canonical reference rig is shaped after.
- `async-openai` — minimal Rust OpenAI client; Rig sits one layer above.
- `mistral.rs` — Rust inference *engine* (in-process), complementary not competitor.
- `kalosm` — Floneum's Rust LLM toolkit; broader (UI, audio) but smaller mindshare.
- `ollama-rs` — single-vendor Rust client for Ollama specifically.

## Internal links
- [[ollama]] — common backend; Rig has an `ollama` provider.
- [[providers]] — broader LLM provider landscape.
- [[rsllm]] — niche Rust LLM streaming tool (very different scope).
- [[programming/rust/ml/ml_in_rust|ml_in_rust]] — broader Rust-ML landscape.
- [[programming/rust/ml/README|programming/rust/ml]] — Rust-ML section landing.
- [[../README|llm]] — parent LLM hub.

## Keywords

`#rig` `#rust` `#llm` `#agents` `#rag` `#embeddings` `#multi-provider`

## References / raw notes

- Repo: <https://github.com/0xPlaygrounds/rig>
- Project docs: <https://docs.rig.rs/>
- API reference: <https://docs.rs/rig-core/latest/rig/>

> Rig is a Rust library for building scalable, modular, and ergonomic
> LLM-powered applications.
