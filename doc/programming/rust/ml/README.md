---
title: Rust ML libraries
keywords: [ml, rust, linfa, candle, burn, cuda, gpu]
status: reviewed
---

# Rust ML libraries

Rust libraries in the ML space, viewed through a Rust-language lens. The
section is deliberately small — for the broader, language-agnostic ML
body of knowledge (frameworks, fundamentals, time-series, RAG, agents,
LLM ops…), see [[../../../ml/README]] and especially
[[../../../ml/frameworks/README]].

## Articles

- [[ml_in_rust]] — landscape overview: classical ML, deep learning,
  utility AI, the picker table.
- [[linfa]] — the scikit-learn-equivalent classical-ML toolkit
  (curated by the rust-ml working group).
- [[cuda|cuda (Rust)]] — Rust crates that talk to NVIDIA GPUs:
  `cudarc` (driver bindings), `Rust-CUDA` (codegen Rust → PTX),
  `cubecl` (`#[cube]` kernels for CUDA + WGPU).

## When to reach for what

| Need                                                    | Reach for                              |
|---------------------------------------------------------|----------------------------------------|
| Classical ML in a Rust service (linear/logistic/k-means/PCA…) | [[linfa]]                        |
| Deep-learning inference of HF / PyTorch checkpoints     | `candle` (HuggingFace)                 |
| Train a smaller neural net entirely in Rust             | `burn` (Tracel) or `dfdx`              |
| PyTorch from Rust (libtorch FFI)                        | `tch-rs`                               |
| Talk to NVIDIA GPUs                                     | [[cuda|cuda (Rust)]] (`cudarc`)        |
| Cross-vendor GPU compute (CUDA + AMD + Apple)           | `cubecl` or raw `wgpu`                 |
| Utility AI for a Bevy game                              | `big-brain`                            |
| Where things stand in general                           | [arewelearningyet.com](http://www.arewelearningyet.com/) |

## See also

- [[../../../ml/README]] — the canonical ML hub (language-agnostic).
- [[../../../ml/frameworks/README]] — ML frameworks across languages.
- [[../../../ml/compilers/cuda|ml/compilers/cuda]] — broader CUDA
  framing (toolchain, kernels, language ports).
- [[../interop/README]] — sibling section on FFI / language bridges
  (relevant when wiring a Python ML notebook to a Rust core).
- [[../README]] — Rust language landing.

## Keywords

`#ml` `#rust` `#linfa` `#candle` `#burn` `#cuda` `#gpu`
