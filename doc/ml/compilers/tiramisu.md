---
title: Tiramisu — polyhedral compiler for high-performance code
main_link: http://tiramisu-compiler.org/
keywords: [tiramisu, polyhedral, compiler, halide, cpu, gpu, fpga, mit]
status: reviewed
review_date: 2026/05/03
---

# Tiramisu — polyhedral compiler for high-performance code

**Main link:** <http://tiramisu-compiler.org/>

## Summary

Tiramisu is a research-grade polyhedral compiler from MIT/INRIA (Riyadh Baghdadi et al.) that compiles a high-level algorithm description, written in C++, down to optimised CPU / GPU / distributed-memory / FPGA targets simultaneously. It sits in the same lineage as Halide (decoupling algorithm from schedule) but uses the **polyhedral model** for loop-nest reasoning, which lets it apply more aggressive transformations (skewing, fusion, tiling, vectorisation) on affine loop nests typical of dense linear algebra and stencils.

## Insight

Tiramisu's pitch is "Halide, but with a polyhedral IR underneath, so the scheduler can be smarter about loops". In practice that means it can target a wider hardware spread (CPU, multi-CPU MPI, CUDA, Xilinx FPGA via HLS) from one source. The honest framing in 2024-2025: it remains a **research project** — useful to study or to cite, occasionally to reproduce a paper result, but not something you reach for to ship a production tensor compiler. For that you'd reach for Apache TVM, MLIR-based compilers, [[xla_accelerated_compiler|XLA]], or [Triton](https://triton-lang.org/) depending on your domain. The polyhedral-compiler space at large (PPCG, Polly inside LLVM, isl, Pluto, AlphaZ) has been overshadowed by ML-driven schedule search (Ansor, MetaSchedule, [[mlgo_llvm|MLGO]]) and tensor-IR efforts (TVM, MLIR, IREE).

When Tiramisu still earns its keep:

- Reproducing the published Tiramisu papers (notably the 2019 PLDI paper showing it matches or beats Halide / Pluto / Intel MKL on selected benchmarks).
- Teaching a compilers course around the polyhedral model with a runnable artefact instead of pure theory.
- A jumping-off point for research that needs a polyhedral front-end without writing one from scratch.

What it isn't: a 2024-grade ML-tensor compiler. The active production action lives in MLIR/IREE/TVM/XLA/Triton, all of which are ML-tensor-shaped and have far bigger communities.

## Similar / related topics

- **Halide** — the canonical "decouple algorithm from schedule" image-processing DSL; Tiramisu's direct predecessor.
- **Apache TVM** — production tensor compiler with auto-schedulers (AutoTVM, Ansor, MetaSchedule).
- **MLIR / IREE** — modern modular compiler infrastructure; many tensor compilers now build on it.
- **Polly** (LLVM) — polyhedral optimisations as an LLVM pass; same techniques, integrated into LLVM.
- **PPCG / Pluto / isl** — older polyhedral toolchains; isl is the SCoP/integer-set library most use under the hood.
- **Triton** (OpenAI) — a different angle: GPU-only, Python-embedded kernel DSL with autotuning.

## Internal links

- [[xla_accelerated_compiler]] — Google's tensor compiler, the production-grade analogue
- [[mlgo_llvm]] — ML-driven heuristics for LLVM, complementary technique
- [[tiny_gpu]] — the hardware end of the same conversation
- [[README|ml/compilers]] — section landing
- [[../../programming/rust/ml/cuda|programming/rust/ml/cuda]] — kernel-level GPU compute from Rust

## Keywords

`#tiramisu` `#polyhedral` `#compiler` `#halide` `#cpu` `#gpu` `#fpga` `#mit`

## References / raw notes

### Project

- Homepage: <http://tiramisu-compiler.org/>
- Repo: <https://github.com/Tiramisu-Compiler/tiramisu>
- Paper (CGO 2019): "Tiramisu: A Polyhedral Compiler for Expressing Fast and Portable Code" — <https://arxiv.org/abs/1804.10694>

### Original raw note

This is a compiler that compiles to CPU, GPU and FPGA from a single source.
It is ML-centric — so keep expectations "in line".
However, this is a different approach from Apache TVM (polyhedral model rather than tensor-IR + auto-schedulers).

### Polyhedral-compiler reading lineage

PPCG → Polly (LLVM) → Pluto → Halide → TVM → Tiramisu → MLIR / IREE.
