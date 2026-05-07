---
title: LLM runtimes — local, hosted, and Rust-side clients
keywords: [runtimes, ollama, vllm, llama-cpp, goose, rig, providers]
status: reviewed
review_date: 2026/05/03
---

# LLM runtimes — local, hosted, and Rust-side clients

How to actually *run* and *call* LLMs. This section gathers the
local runtimes (Ollama and friends), production inference servers,
the cloud-provider landscape, the Rust-side clients (`rig`,
`rsllm`, `ollama-rs`), and Block's `goose` agent runtime.

## Articles

### Local desktop runtimes

- [[ollama]] — the canonical "run a model on your laptop" tool;
  Modelfile abstraction, OpenAI-compatible endpoint, llama.cpp
  underneath.
- [[ollama_ubuntu_share]] — practical recipe for sharing a single
  Ollama instance over LAN from an Ubuntu host (systemd override,
  ufw, SSH tunnel / Tailscale).

> Mentioned-but-no-article: **LM Studio** (GUI on top of llama.cpp),
> **`llama.cpp`** (the C++ engine Ollama wraps), **mlx-lm** (Apple
> Silicon native), **Jan** / **GPT4All** (desktop shells).

### Production inference servers

> Mentioned but not yet articles: **vLLM** (PagedAttention,
> high-throughput batch serving), **SGLang** (RadixAttention,
> structured-output focus), **TensorRT-LLM** (NVIDIA's optimised
> path), **TGI** (HuggingFace Text Generation Inference). Reach for
> these when you outgrow Ollama's single-request model and need
> real concurrency.

### Cloud / managed providers

- [[providers]] — provider matrix and the broader 2024-2025 landscape
  (frontier vendors, inference clouds, aggregators like OpenRouter
  and LiteLLM).

### Rust-side clients & frameworks

- [[rig]] — `rig` crate from 0xPlaygrounds: the Rust answer to
  LangChain (multi-provider client + agents + tools + embeddings).
- [[rsllm]] — niche Rust pipeline (Twitch bot + NDI + Candle) on
  Apple Silicon; honest "use only if your problem matches its
  shape" framing.

### Agent runtimes

- [[goose]] — Block's open-source AI agent CLI; MCP-first,
  provider-agnostic, the open answer to Claude Code / Cursor / Codex.
- [[extensions]] — Goose's extensions, sessions, and `.goosehints`
  (the surface area you actually use day to day).

## Decision shortcut

| You want…                                                  | Reach for                  |
|------------------------------------------------------------|----------------------------|
| Run a local model on a laptop in 30 seconds                | [[ollama]]                 |
| Share an Ollama instance over LAN                          | [[ollama_ubuntu_share]]    |
| GUI for browsing/running local models                      | LM Studio (no article)     |
| Maximum throughput / batched serving                       | vLLM / SGLang / TensorRT-LLM (no article) |
| Apple-Silicon-native inference (faster than llama.cpp sometimes) | mlx-lm (no article)  |
| Multi-vendor cloud LLM call without lock-in                | [[providers]] → OpenRouter / LiteLLM |
| LangChain-shaped agents/RAG/tools in Rust                  | [[rig]]                    |
| Open AI-agent CLI, alternative to Claude Code/Cursor/Codex | [[goose]]                  |
| Add tools/MCP-servers to Goose                             | [[extensions]]             |
| Rust crate to call only Ollama                             | `ollama-rs` (mentioned in [[ollama]]) |
| Twitch bot with image-gen + TTS over NDI on Apple Silicon  | [[rsllm]] (niche)          |

## Cross-section see-also

- [[../README|llm]] — parent LLM hub.
- [[../models/README|llm/models]] — what to actually run on these runtimes.
- [[../../tools/README|ml/tools]] — sister section: Candle, MLX, transformers, etc.
- [[../../mcp/README|mcp]] — Model Context Protocol; how Goose & friends talk to tools.
- [[../../agents/README|agents]] — broader agent-framework landscape.
- [[../../frameworks/README|ml/frameworks]] — DSPy, LangGraph, agno/phidata (Python side).
- [[programming/rust/ml/README|programming/rust/ml]] — the broader Rust-ML landscape (Candle/burn/tch/linfa).

## Keywords

`#runtimes` `#ollama` `#vllm` `#llama-cpp` `#goose` `#rig` `#providers`
