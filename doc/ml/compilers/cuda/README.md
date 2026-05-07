---
title: CUDA — NVIDIA's parallel-computing platform (broader hub)
keywords: [cuda, nvidia, gpu, gpgpu, cudnn, cublas, triton, ptx, hopper, blackwell, rocm]
status: reviewed
---

# CUDA — NVIDIA's parallel-computing platform (broader hub)

This is the broader CUDA landing page in the vault — the place to start for anything CUDA-shaped, whether you came from the Rust-CUDA bindings angle ([[../../../programming/rust/ml/cuda|programming/rust/ml/cuda]]), the LLM-writes-kernels angle ([[kevin]]), the ML-compilers angle ([[../README|ml/compilers]]), or the accelerator-hardware angle ([[../../../dpu/README|dpu]]).

## What is CUDA

CUDA (originally "Compute Unified Device Architecture", a name NVIDIA quietly stopped expanding around 2008) is NVIDIA's parallel-computing platform first released in **2007**. It introduced the now-universal CPU-GPU programming model — host code in C/C++ launches kernels onto a GPU device with the `kernel<<<grid, block>>>(args)` triple-bracket syntax — and gradually extended into a full SDK with libraries, compilers (`nvcc`, `nvrtc`), profilers (`nsys`, `ncu`), debuggers (`cuda-gdb`), runtime APIs and a unified-memory model.

The current major version line is **CUDA 12.x** (12.0 shipped late 2022; 12.6+ targets the Hopper/H100/H200 and Blackwell/B100/B200 architectures with Transformer Engine, FP8/FP4, and TMA async copies). NVIDIA ships a new major release roughly yearly, with backwards-compatibility forward across driver versions via the **forward-compatibility package** and **MIG** (multi-instance GPU) for partitioning.

## The library ecosystem (what you actually use)

You rarely write raw CUDA C++ for production ML. The libraries doing the work:

| Library | What it does | Typical caller |
|---------|--------------|----------------|
| **cuBLAS / cuBLASLt** | GEMM and BLAS-3 ops, the core of every transformer | PyTorch, TensorFlow, JAX, XLA, Triton kernels |
| **cuDNN** | Deep-learning primitives (conv, attention, normalisation, RNN) | PyTorch, TensorFlow, ONNX-Runtime |
| **cuFFT** | Fast Fourier Transforms | scientific computing, signal processing |
| **cuSPARSE / cuSOLVER** | Sparse linear algebra and dense solvers | sci-comp, GNNs |
| **CUTLASS** | Templated CUDA C++ for hand-tuning GEMMs and convs (the modern way to write a custom op) | kernel engineers, FlashAttention authors |
| **NCCL** | Multi-GPU / multi-node collective comms (allreduce, allgather) | every distributed training framework |
| **Thrust / CUB** | STL-shaped parallel algorithms (sort, scan, reduce) on GPU | numerical pipelines |
| **TensorRT** | Inference graph optimiser + runtime | production ML serving |
| **cuQuantum** | Quantum-computing simulation primitives | quantum software stacks |
| **cuGraph / RAPIDS** | GPU-accelerated graph and DataFrame processing | data engineering |

## Higher-level kernel languages (the modern "don't write raw CUDA" path)

- **Triton** (OpenAI, 2021+) — Python-embedded DSL that emits PTX. The de-facto modern way to write a custom GPU kernel. PyTorch 2's `torch.compile` (TorchInductor) emits Triton for fused ops.
- **CUDA C++ + CUTLASS** — the path of choice when Triton's autotuning isn't enough; FlashAttention-2/3 and most production attention kernels live here.
- **cuTeDSL / CuTe** — NVIDIA's tile-based DSL inside CUTLASS for expressing layouts; the modern way to think about Hopper/Blackwell tensor cores.
- **JAX / XLA** — write Pythonic numpy, get fused GPU code via [[../xla_accelerated_compiler|XLA]].
- **PyTorch + `torch.compile`** — same idea, the PyTorch-native path.

## The 2024-2025 hardware reality

- **Hopper (H100, H200)** — FP8 + Transformer Engine + TMA + 4th-gen Tensor Cores; the workhorse of 2024 LLM training.
- **Blackwell (B100, B200, GB200 NVL72)** — FP4 + 5th-gen Tensor Cores + bigger NVLink domains; ramping through 2025.
- **A800 / H800 / H20** — China-export-restricted variants with reduced NVLink/FP64; periodically tightened by US export controls.
- **DGX Cloud / NVLink Fusion** — NVIDIA selling the rack-scale interconnect story, not just the chip.
- **Grace + Hopper / Blackwell ARM CPU + GPU SoCs** — increasingly the high-end form factor.

## CUDA vs the alternatives

- **AMD ROCm + HIP** — CUDA-shape API, can be ported with `hipify`. ROCm 6 ships with PyTorch first-class support; the MI300X is the closest credible competitor to H100/H200 on raw FLOPs and HBM, although the software stack still trails CUDA's polish.
- **Apple Metal Performance Shaders + MPS Graph + MLX** — the Apple Silicon path; MLX is the modern Pythonic stack.
- **Intel oneAPI / SYCL / Intel GPU Max** — the Khronos open-standard angle; smaller mindshare in ML.
- **Google TPU + XLA** — vertically integrated; the only path for TPUs.

The honest 2025 picture: CUDA is still the default and the deepest software stack by a wide margin. ROCm is closing on parity for inference and starting to be credible for training. Apple MLX is delightful for on-device but not in the H100-replacement conversation.

## Articles

- [Kevin (Cognition CUDA-kernel model)](kevin.md) — Kevin-32B, multi-turn RL fine-tune for writing CUDA kernels.

## See also

- [[../README|ml/compilers]] — broader ML-compilers section (XLA, MLGO, Tiramisu, tiny-gpu)
- [[../xla_accelerated_compiler|xla_accelerated_compiler]] — what compiles to CUDA (when you don't write the kernels)
- [[../tiny_gpu]] — for understanding the GPU you're emitting code for
- [[../../../programming/rust/ml/cuda|programming/rust/ml/cuda]] — Rust-side CUDA bindings (`cudarc` / `Rust-CUDA` / `cubecl`)
- [[../../../dpu/README|dpu]] — broader accelerator landscape (FPGAs, DPUs, IPUs)
- [[../../llm/README|ml/llm]] — what consumes most CUDA cycles in 2025

## External canonical references

- CUDA Toolkit homepage: <https://developer.nvidia.com/cuda-toolkit>
- CUDA C++ Programming Guide: <https://docs.nvidia.com/cuda/cuda-c-programming-guide/>
- Triton: <https://triton-lang.org/>
- CUTLASS: <https://github.com/NVIDIA/cutlass>
- ROCm (the AMD alternative): <https://rocm.docs.amd.com/>

## Keywords

`#cuda` `#nvidia` `#gpu` `#gpgpu` `#cudnn` `#cublas` `#triton` `#ptx` `#hopper` `#blackwell` `#rocm`
