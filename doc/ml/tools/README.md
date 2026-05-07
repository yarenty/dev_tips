---
title: ML tools — frameworks, runtimes, IDEs, and workflow systems
keywords: [ml-tools, rust-deep-learning, ai-ide, robotics, workflow-automation]
status: reviewed
---

# ML tools — frameworks, runtimes, IDEs, and workflow systems

User-facing tools and runtimes for ML / AI work. This section is the **ML-tooling hub**, distinct from:

- [[../frameworks/README|ml/frameworks]] — agent / LLM orchestration libraries (DSPy, LangGraph, MindsDB, …) and AutoML.
- [[../../programming/rust/ml/README|programming/rust/ml]] — the broader pure-Rust ML library landscape (linfa, smartcore, big-brain, candle/burn/tch from the Rust-ecosystem angle, …).
- [[../README|ml]] — the parent landing.

If you're picking a tool for a specific job, the table below is the fast path; the thematic groups underneath give the why.

## Decision shortcut

| You want to… | Reach for | Notes |
|---|---|---|
| Train a deep-learning model in pure Rust | [[burn]] | most general; backend-agnostic (WGPU/Cubecl/Metal/LibTorch/Candle/NdArray); training-first |
| Inference an HF model from a small Rust binary | [[candle]] | PyTorch-like API; tiny binaries; `candle-transformers` model zoo |
| Run a PyTorch model from Rust without rewriting | [[tch]] | wraps libtorch C++; heaviest dep, fullest op set |
| Compile-time tensor shape checking in Rust | [[dfdx]] | const-generic API; less actively maintained 2024+ |
| Deploy ONNX/TF on edge / embedded CPU in pure Rust | [[tract]] | Sonos-tested; CPU only; no GPU |
| AI-first IDE with Composer / Tab / Chat | [[cursor]] | closed; VS Code fork; the polished default |
| Self-host a Copilot-alternative on your own GPUs | [[tabby]] | Apache-2.0; OpenAPI; air-gap-friendly |
| Modern robot learning (imitation + foundation models) | [[lerobot]] | HF; PyTorch; ACT / Diffusion Policy / SmolVLA / π0 |
| Multimodal training-data store + vector DB hybrid | [[deeplake]] | Activeloop; v4 rewrite 2024; LanceDB has more momentum |
| Self-hostable Zapier-alternative with AI-agent nodes | [[n8n]] | fair-source SUL; LangChain nodes built in |
| Open-source deep-research agent | [[webthinker]] | LRM-driven; the open answer to OpenAI Deep Research |

## Thematic groups

### Rust deep-learning frameworks

- [[burn]] — Tracel-AI's backend-agnostic pure-Rust DL framework; the most ambitious of the bunch.
- [[candle]] — HuggingFace's minimalist Rust ML framework; PyTorch-like; serverless-shaped.
- [[tch]] — Rust bindings to PyTorch's libtorch C++ runtime; the most mature Rust→PyTorch path.
- [[dfdx]] — pure-Rust DL with **compile-time tensor shape checking** via const generics.
- [[tract]] — Sonos's pure-Rust ONNX / TensorFlow / NNEF inference engine; CPU only, edge-friendly.

### AI-IDE / coding tools

- [[cursor]] — Anysphere's AI-first IDE; the polished closed default.
- [[tabby]] — TabbyML self-hosted code-completion server; the open / on-prem answer.

### Robotics / data

- [[lerobot]] — HuggingFace's robot-learning library; ACT / Diffusion Policy / SmolVLA / π0.
- [[deeplake]] — Activeloop Deep Lake; vector DB + ML-native dataset store.

### Workflow / automation

- [[n8n]] — n8n.io fair-source workflow automation; LangChain-based AI-agent nodes built in.

### Web research / agents

- [[webthinker]] — RUC NLPIR's LRM-powered open-source deep-research agent.

## Cross-section see-also

- [[../README|ml]] — the parent landing.
- [[../llm/README|llm]] — LLM hub (models, runtimes, prompting, inspection); P5.AD reviewed.
- [[../frameworks/README|frameworks]] — orchestration libraries (DSPy, LangGraph, MindsDB, AutoML, …); P5.AB reviewed.
- [[../../programming/rust/ml/README|programming/rust/ml]] — the broader Rust-ML library landscape; P5.Y reviewed.
- [[../agents/coding_agents|agents/coding_agents]] — IDE-agent landscape with form-factor picker; P5.AF reviewed.
- [[../agents/README|agents]] — autonomous LLM-driven systems; P5.AF reviewed.
- [[../voice/README|voice]] — voice-AI hub (parallel agent's review).
- [[../orchestration/README|ml/orchestration]] — top-level orchestration section (parallel agent's review).
- [[../rag/README|rag]] — RAG section; cross-link for tools used in retrieval pipelines (Deep Lake, Qdrant).

## Keywords

`#tools` `#ml` `#rust` `#deep-learning` `#ai-ide` `#robotics` `#workflow-automation`
