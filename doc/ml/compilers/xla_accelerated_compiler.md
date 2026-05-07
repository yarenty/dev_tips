---
title: XLA — Google's Accelerated Linear Algebra compiler
main_link: https://openxla.org/
keywords: [xla, openxla, jax, tensorflow, pytorch, tpu, mlir, stablehlo, pjrt]
status: reviewed
---

# XLA — Google's Accelerated Linear Algebra compiler

**Main link:** <https://openxla.org/>

## Summary

XLA (Accelerated Linear Algebra) is Google's domain-specific compiler for linear algebra and tensor ops, used to take a model from JAX, TensorFlow or PyTorch and lower it through StableHLO into fused, hardware-specific code for CPU, GPU, and TPU. Originally built inside TensorFlow, since 2023 it lives under the **OpenXLA** umbrella with open governance — including the related **PJRT** (uniform device runtime), **StableHLO** (stable IR for portability), and **IREE** (alternative MLIR-based runtime). It's the de-facto compiler stack underneath JAX and Google's internal TPU production workloads.

## Insight

The XLA pitch is straightforward: take a tensor program, fuse adjacent ops to cut memory traffic (the dominant cost in modern accelerators), specialise to actual tensor shapes, and emit code for the device. The honest 2024-2025 picture:

- **JAX uses XLA as its only backend.** If you write JAX, you are an XLA user whether you know it or not. `jit` triggers an XLA compilation.
- **TensorFlow uses XLA as a `tf.function(jit_compile=True)` opt-in.** Default TF graph execution still goes through the older XLA-less path for many ops.
- **PyTorch's XLA story is the `torch_xla` package**, primarily aimed at TPUs. PyTorch's main GPU compilation story moved to TorchInductor (part of `torch.compile`) which is a different codebase. `torch.export` + StableHLO is the bridge.
- **Most non-Google-scale users see XLA as "the thing that makes JAX fast on a TPU"**. On NVIDIA GPUs, the typical comparison is XLA vs `torch.compile` vs TensorRT vs hand-rolled Triton kernels — XLA is competitive on whole-graph compilation, less competitive on hand-tuned single kernels.

OpenXLA's organisational shift (2023, Linux Foundation governance, multi-vendor steering with NVIDIA, AMD, Intel, AWS, Meta) was real but the contributor mix is still very Google-heavy. StableHLO has succeeded as a portable IR (Apple's MLX, Modular's MAX, IREE all consume it).

Where to reach for it:

- You are training/serving on TPU — XLA is the only path.
- You are on JAX — already done.
- You want whole-graph compilation on GPU and your model is shape-stable — try it, but `torch.compile` may be a faster path on PyTorch.

Where to skip:

- Highly dynamic shapes (re-compilation cost dominates).
- A single hot kernel where Triton or hand-tuned CUDA wins.
- A small model where eager-mode latency already meets your budget.

## Similar / related topics

- **TorchInductor (`torch.compile`)** — PyTorch 2's whole-graph GPU compiler, the practical alternative on NVIDIA.
- **TensorRT** — NVIDIA's inference-side graph optimiser, narrower scope, hand-tuned kernel library underneath.
- **Apache TVM / Triton** — alternative tensor compilers, different sweet spots.
- **IREE** — MLIR-based runtime under the OpenXLA umbrella; targets edge / mobile / Vulkan well.
- **MLIR / StableHLO / PJRT** — the IR + runtime infrastructure XLA is built on.
- **Modular MAX / Mojo** — a 2024-grade non-XLA alternative tensor stack.

## Internal links

<!-- reviewed -->
- [[mlgo_llvm]] — Google's other ML-driven compiler effort, but at the LLVM-IR level
- [[tiramisu]] — research-grade polyhedral compiler in the same broad space
- [[tiny_gpu]] — the hardware end of the same stack
- [[cuda/README|cuda]] — when XLA targets NVIDIA, this is what it eventually emits for
- [[README|ml/compilers]] — section landing
- [[../../programming/rust/ml/ml_in_rust|ml_in_rust]] — Rust-side ML stack overview

## Keywords

`#xla` `#openxla` `#jax` `#tensorflow` `#pytorch` `#tpu` `#mlir` `#stablehlo` `#pjrt`

## References / raw notes

### Project

- Homepage: <https://openxla.org/>
- Repo: <https://github.com/openxla/xla>
- StableHLO spec: <https://openxla.org/stablehlo>
- PJRT: <https://openxla.org/pjrt>
- IREE: <https://iree.dev/>

### Project's pitch

> **XLA (Accelerated Linear Algebra)** is an open-source machine learning (ML) compiler for GPUs, CPUs, and ML accelerators.
>
> The XLA compiler takes models from popular ML frameworks such as PyTorch, TensorFlow, and JAX, and optimizes them for high-performance execution across different hardware platforms including GPUs, CPUs, and ML accelerators.

### Get started — framework docs

- JAX: <https://jax.readthedocs.io/en/latest/jit-compilation.html>
- TensorFlow: <https://www.tensorflow.org/xla>
- PyTorch (`torch_xla`): <https://pytorch.org/xla/>

### Build from source (Docker)

```sh
git clone https://github.com/openxla/xla && cd xla
docker run --name xla -w /xla -it -d --rm -v "$PWD":/xla tensorflow/build:latest-python3.9 bash
docker exec xla ./configure
docker exec xla bazel test xla/examples/axpy:stablehlo_compile_test --nocheck_visibility --test_output=all
```

The first build takes a while because it builds the entire stack (MLIR, StableHLO, XLA, etc.).
