---
title: Ollama — run LLMs locally with one command
main_link: https://ollama.com/
keywords: [ollama, local-llm, llama-cpp, modelfile, openai-compatible, gguf, runtime]
status: reviewed
review_date: 2026/05/03
---

# Ollama — run LLMs locally with one command

**Main link:** <https://ollama.com/>

## Summary

Ollama is the de-facto "Docker for LLMs": a single Go binary that
wraps `llama.cpp` behind a `pull / run / serve` lifecycle, a
`Modelfile` abstraction (image-shaped recipes for prompt templates,
system prompts, parameters), and an HTTP server on
`127.0.0.1:11434` that exposes both a native API and an
OpenAI-compatible `/v1/chat/completions` endpoint. It ships a
curated model library (a subset of HuggingFace, in GGUF) and handles
the GPU/Metal/CUDA backend selection automatically. As of 2024-2025
Ollama also offers a hosted "Ollama Cloud" tier for larger models
that don't fit on a laptop.

## Insight

Reach for Ollama when you want to *run* a model on your own machine
with the least friction — not when you want maximum throughput
(use vLLM/SGLang/TensorRT-LLM) or the absolute lowest-level control
(use raw `llama.cpp`). Three things to know:

- **It is `llama.cpp` underneath.** GGUF quantisation, the same
  CPU/GPU backend matrix, the same context-window limits. Ollama
  adds the registry, the Modelfile DSL, and the always-on server.
- **The model library is curated, not the whole HuggingFace.** If a
  model isn't in `ollama.com/library`, you can pull a GGUF from HF
  and write a `Modelfile FROM ./model.gguf` — but it's not as
  one-click as the official entries suggest.
- **No authentication out of the box.** The default bind is
  `127.0.0.1` precisely because anyone who can reach `:11434` owns
  the box. To share it on a LAN or over the internet, see
  [[ollama_ubuntu_share]].

Comparison sketch:

| Tool       | Niche                                                   |
|------------|---------------------------------------------------------|
| Ollama     | "I want a model running in 30 seconds on macOS/Linux"   |
| LM Studio  | Same, but with a GUI and a model browser                |
| `llama.cpp`| Raw — best when you want to tune flags or embed the lib |
| `mlx-lm`   | Apple Silicon native, sometimes faster than llama.cpp   |
| Jan / GPT4All | Desktop-app shells around llama.cpp, more end-user-y |
| vLLM / SGLang | Production inference servers (PagedAttention, batching) |

## Similar / related topics

- LM Studio — GUI-first local LLM runner; same llama.cpp underneath.
- `llama.cpp` — the C++ inference engine Ollama wraps.
- mlx-lm — Apple Silicon native runner using the MLX framework.
- vLLM — high-throughput Python inference server (PagedAttention).
- ollama-rs — Rust client for the Ollama HTTP API.

## Internal links
- [[ollama_ubuntu_share]] — share an Ollama instance over LAN from Ubuntu.
- [[providers]] — Ollama as one of Goose's LLM providers.
- [[rig]] — Rust LLM-app framework with an Ollama backend.
- [[../models/llama|models/llama]] — the Llama family Ollama is named after.
- [[../models/deepseek|models/deepseek]] — DeepSeek-R1 (incl. the Goose-tooling variant).
- [[../README|llm]] — parent LLM hub.
- [[../../tools/README|ml/tools]] — sibling section: Candle, MLX, transformers.
- [[programming/rust/ml/ml_in_rust|ml_in_rust]] — Rust ML landscape (Candle/burn/tch).

## Keywords

`#ollama` `#local-llm` `#llama-cpp` `#modelfile` `#openai-compatible` `#gguf` `#runtime`

## References / raw notes

Install (Linux):

```sh
curl -fsSL https://ollama.com/install.sh | sh
```

Default endpoint: `http://127.0.0.1:11434/`.

### Models

- Library: <https://ollama.com/search>
- DeepSeek-R1 with tool-calling support (Michael Neale's variant for Goose):
  ```sh
  ollama run michaelneale/deepseek-r1-goose
  ```
- `kirito1/qwen3-coder` — Qwen3 Coder build with FIM (fill-in-the-middle); see
  <https://ollama.com/kirito1/qwen3-coder>.

### Rust client

- [`pepperoni21/ollama-rs`](https://github.com/pepperoni21/ollama-rs) —
  Rust crate; check the `examples/` folder, especially the streaming
  example.

### Sharing over LAN

See [[ollama_ubuntu_share]] for the full recipe (`OLLAMA_HOST=0.0.0.0`,
systemd override, ufw, SSH tunnel / Tailscale alternatives).
