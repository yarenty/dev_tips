---
title: tiny-gpu — a minimal GPU in 15 Verilog files
main_link: https://github.com/adam-maj/tiny-gpu
keywords: [tiny-gpu, gpu, verilog, education, gpgpu, hardware, rtl]
status: reviewed
---

# tiny-gpu — a minimal GPU in 15 Verilog files

**Main link:** <https://github.com/adam-maj/tiny-gpu>

## Summary

Adam Majmudar's `tiny-gpu` is an educational, minimal GPU implementation in **<15 fully-documented Verilog files**, built from the ground up to teach how a modern general-purpose GPU works. It implements a small ISA, an SM-style core with multiple threads, a memory controller, and ships working matrix-add / matrix-multiply kernels with full simulation traces. It is deliberately **not** a graphics GPU and is **not** synthesised for FPGAs by default — it's a teaching artefact for understanding GPGPU/TPU architecture from first principles.

## Insight

Reach for `tiny-gpu` when you want to internalise *why* a GPU has the shape it does (warps, SIMT execution, lockstep PCs, memory coalescing, kernel-launch grid/block semantics) by reading something small enough to hold in your head, rather than ploughing through real RTL of an Ampere SM. The repo's documentation is the headline asset — there are clear architecture diagrams of the chip-level / core-level / thread-level hierarchies, an explained ISA, and traceable test runs.

What it is **not**:

- A production GPU. No texture units, no rasteriser, no graphics pipeline — it focuses on the GPGPU/ML-accelerator subset (matrix kernels), reflecting the trend that most modern GPU silicon is doing GEMMs not graphics.
- A synthesisable FPGA-ready design. There's no PCIe interface, no off-chip DRAM controller, no clock-domain crossing work — running on real silicon is left as an exercise.
- A drop-in for a CUDA / Triton / OpenCL stack. There is no toolchain to lower a high-level kernel language down to `tiny-gpu` ISA; you write the ISA by hand.

Where it sits in the space of "build-it-yourself hardware" projects: `tiny-gpu` is to GPUs what [picorv32](https://github.com/YosysHQ/picorv32) and [SERV](https://github.com/olofk/serv) are to CPUs, what [LiteX](https://github.com/enjoy-digital/litex) is to SoC integration, and what [nand2tetris](https://www.nand2tetris.org/) is to the whole stack — pedagogical, minimal, with the educational scaffolding being the entire point.

## Similar / related topics

- **picorv32 / SERV / VexRiscv** — minimal RISC-V CPU cores in the same "small, readable, educational" tradition.
- **LiteX** — Migen/Python-based FPGA SoC framework; productive but bigger than `tiny-gpu`.
- **MIAOW / FlexGripPlus** — open-source AMD/Nvidia-shape GPU clones; more complete, far heavier to read.
- **Vortex** — CalTech RISC-V-based GPGPU, FPGA-synthesisable; the "next step up" from `tiny-gpu`.
- **nand2tetris / Computer Enhance / Crafting Interpreters** — same pedagogical genre, different domains.
- **Triton (OpenAI)** — opposite end of the stack: a high-level kernel language that *targets* real GPUs.

## Internal links

<!-- reviewed -->
- [[README|ml/compilers]] — the broader ML-compilers section
- [[cuda/README|cuda]] — the production GPU side of the same conversation
- [[xla_accelerated_compiler]] — what runs *on* a GPU when you don't write the kernels yourself
- [[../../dpu/README|dpu]] — the DPU/accelerator broader landscape
- [[../../programming/rust/ml/cuda|programming/rust/ml/cuda]] — the Rust-CUDA side

## Keywords

`#tiny-gpu` `#gpu` `#verilog` `#education` `#gpgpu` `#hardware` `#rtl`

## References / raw notes

### Project

- Repo: <https://github.com/adam-maj/tiny-gpu>
- ~15 Verilog files, fully documented architecture + ISA, working matrix add / multiply kernels with simulation traces.

### Author's framing

> tiny-gpu is a minimal GPU implementation optimized for learning about how GPUs work from the ground up.
>
> Specifically, with the trend toward general-purpose GPUs (GPGPUs) and ML-accelerators like Google's TPU, tiny-gpu focuses on highlighting the general principles of all of these architectures, rather than on the details of graphics-specific hardware.

### Architecture diagrams (in repo)

- Chip-level: <https://github.com/adam-maj/tiny-gpu/blob/master/docs/images/gpu.png>
- Core-level: <https://github.com/adam-maj/tiny-gpu/raw/master/docs/images/core.png>
- Thread-level: <https://github.com/adam-maj/tiny-gpu/raw/master/docs/images/thread.png>
