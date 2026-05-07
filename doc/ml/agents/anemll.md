---
title: Anemll — Apple Neural Engine LLM runtime
main_link: https://github.com/Anemll/Anemll
keywords: [anemll, ane, apple-neural-engine, coreml, on-device, runtime]
status: reviewed
review_date: 2026/05/03
---

# Anemll — Apple Neural Engine LLM runtime

**Main link:** <https://github.com/Anemll/Anemll>

## Summary

ANEMLL (pronounced "animal") is an open-source pipeline for porting Llama-style LLMs to the **Apple Neural Engine** (ANE) via CoreML — i.e. on-device inference on iPhone / iPad / Mac that runs on the dedicated NPU silicon rather than the CPU or GPU. It covers the full toolchain from Hugging Face checkpoint → CoreML conversion → Swift / C++ inference samples for iOS and macOS apps. The project's selling point is **edge-device privacy**: the model stays on the user's device, no network round-trip required.

## Insight

**This is misfiled in `agents/`** — Anemll is an **on-device LLM runtime**, not an agent framework. It's the spiritual cousin of `llama.cpp`, `mlx-lm`, and Ollama, but specialised for the ANE silicon (the small dedicated neural co-processor on Apple Silicon, distinct from the GPU). The article lives here for hyperlink stability; the proper home is the section landing **[[../llm/runtimes/README|llm/runtimes]]** (P5.AD reviewed).

The honest framing for when to reach for it:

- **Sweet spot.** You're shipping a Llama-architecture model inside a native Apple app and you want the model to run on the NPU rather than the GPU (saves battery, frees the GPU for graphics, slightly lower-latency for small models).
- **Vs alternatives on Apple Silicon.** [`mlx-lm`](https://github.com/ml-explore/mlx-lm) (MLX-based, runs on the GPU and unified memory — wider model coverage, much more momentum) and `llama.cpp` Metal backend (CPU + GPU, the de facto local-LLM runtime) both target the Mac GPU rather than the ANE. The ANE is more power-efficient but less flexible — only certain ops are accelerated and the conversion path (CoreML) is more constrained. For 7B-class models on M-series Macs, `mlx-lm` is the pragmatic choice; Anemll wins on iPhone/iPad where battery and thermals dominate.
- **Gotchas.** ANE is op-coverage-limited; not all model architectures convert cleanly. CoreML conversion is a build-time step (slow), not a runtime swap. Quantisation support is the active development area. The project is small-team and the model zoo is curated, not exhaustive.

If you're building an *agent* and ended up here looking for an Apple-on-device LLM, you probably want to compose Anemll (or `mlx-lm`) with a Swift agent loop yourself — there's no Anemll-flavoured agent framework.

## Similar / related topics

- **`mlx-lm`** — Apple's MLX-based LLM runtime; targets the unified-memory GPU, much wider model coverage.
- **`llama.cpp` Metal backend** — the de facto local-LLM runtime on Mac (CPU + GPU, not ANE).
- **CoreML Tools** — Apple's official PyTorch/TF → CoreML converter that Anemll wraps.
- **whisper.cpp / `sherpa-onnx`** — analogous on-device runtimes for speech.
- **Ollama** — the cross-platform "Docker for LLMs" runtime; Mac variant uses Metal not ANE.

## Internal links

- [[README]] — agents section landing (this article is misfiled here).
- [[../llm/runtimes/README|llm/runtimes]] (P5.AD) — **canonical home in spirit**: section on local desktop and on-device LLM runtimes.
- [[../llm/runtimes/ollama|ollama]] (P5.AD) — cross-platform local-LLM runtime for comparison.
- [[../llm/models/llama|llama]] (P5.AD) — the Llama family (Anemll's primary target architecture).

## Keywords

`#anemll` `#ane` `#apple-neural-engine` `#coreml` `#on-device` `#runtime`

## References / raw notes

- Repo: <https://github.com/Anemll/Anemll>
- Goal (from the README): provide a fully open-source pipeline from model conversion to inference for common LLM architectures running on ANE, enabling on-device inference for low-power applications on edge devices.
- Aims: flexible library/framework to port LLMs to ANE directly from Hugging Face models; on-device examples for iOS and macOS Swift / C / C++ applications.
- Apple's Core ML Tools (the underlying converter): <https://github.com/apple/coremltools>
- Apple's *Deploying Transformers on the Apple Neural Engine* (2022 ML research post): <https://machinelearning.apple.com/research/neural-engine-transformers> — the architectural background for ANE-targeting LLMs.
- For the broader ANE story: WWDC sessions on Core ML Performance Reports.
