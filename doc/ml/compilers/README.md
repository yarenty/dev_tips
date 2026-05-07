---
title: ML compilers — XLA, MLGO, Tiramisu, tiny-gpu, CUDA
keywords: [ml-compilers, xla, mlgo, tiramisu, tiny-gpu, cuda, llvm, mlir, kernel-generation]
status: reviewed
review_date: 2026/05/03
---

# ML compilers — XLA, MLGO, Tiramisu, tiny-gpu, CUDA

ML compilers and kernel-generation tooling — what turns a high-level model description into the actual instructions a GPU/TPU/CPU/FPGA runs. Spans tensor-graph compilers (XLA), classical compiler optimisation guided by ML (MLGO), polyhedral research compilers (Tiramisu), educational hardware (tiny-gpu), and the broader CUDA hub.

## Articles — modern ML compilers

- [XLA](xla_accelerated_compiler.md) — Google's tensor-graph compiler under the OpenXLA umbrella; the JAX/TF/Torch-XLA backend, the only TPU path.
- [Tiramisu](tiramisu.md) — MIT/INRIA polyhedral compiler targeting CPU/GPU/FPGA from one source; research-grade.
- [MLGO](mlgo_llvm.md) — Google's framework for replacing LLVM compiler heuristics with reinforcement-learned policies (in-tree since ~2021).
- [tiny-gpu](tiny_gpu.md) — minimal Verilog GPU implementation in <15 files; educational, not production.

## Subsections

- [CUDA hub](cuda/README.md) — broader CUDA landing (toolchain, libraries, ecosystem, alternatives) since multiple cross-references point here.
  - [Kevin (CUDA-kernel-writing model)](cuda/kevin.md) — Cognition AI's 32B model trained with multi-turn RL.

## Decision shortcuts

| You want… | Reach for |
|-----------|-----------|
| Compile a JAX/TF/PyTorch model for TPU | [[xla_accelerated_compiler]] (the only path) |
| Compile a PyTorch model for NVIDIA GPU | `torch.compile` (TorchInductor → Triton) usually beats XLA in 2025 |
| ML-driven optimisation inside `clang`/`opt` | [[mlgo_llvm]] |
| Polyhedral loop transformations for affine loops | [[tiramisu]] (research) or LLVM Polly (in-tree) |
| Understand what a GPU actually is | [[tiny_gpu]] (Verilog) + [[cuda/README|CUDA hub]] |
| Write a custom GPU kernel | Triton (Python DSL) or CUDA C++ + CUTLASS (production) |
| LLM that writes CUDA kernels for you | [[cuda/kevin|Kevin-32B]] (still very much a research tool) |
| Rust-side CUDA bindings | [[../../programming/rust/ml/cuda|programming/rust/ml/cuda]] |

## See also

- [[../frameworks/README|ml/frameworks]] — what *uses* these compilers (PyTorch, TensorFlow, JAX, Burn, Candle)
- [[../tools/README|ml/tools]] — adjacent ML tooling
- [[../llm/README|ml/llm]] — what consumes most compiled GPU cycles in 2025
- [[../../programming/rust/ml/cuda|programming/rust/ml/cuda]] — Rust-CUDA crates (cudarc / Rust-CUDA / cubecl)
- [[../../programming/rust/ml/README|programming/rust/ml]] — Rust-side ML crates (linfa / candle / burn)
- [[../../dpu/README|dpu]] — broader accelerator landscape (FPGAs, DPUs, IPUs)

## Keywords

`#ml-compilers` `#xla` `#mlgo` `#tiramisu` `#tiny-gpu` `#cuda` `#llvm` `#mlir` `#kernel-generation`
