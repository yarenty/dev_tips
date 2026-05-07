---
title: MLGO — ML-guided compiler optimisations for LLVM
main_link: https://github.com/google/ml-compiler-opt
keywords: [mlgo, llvm, compiler-optimisation, reinforcement-learning, ml-for-systems, google]
status: reviewed
---

# MLGO — ML-guided compiler optimisations for LLVM

**Main link:** <https://github.com/google/ml-compiler-opt>

## Summary

MLGO ("Machine-Learning Guided Compiler Optimizations") is Google's framework for replacing hand-tuned heuristics inside LLVM with policies learned via reinforcement learning. The two flagship demonstrations are **inlining-for-size** (smaller binaries) and **register allocation** (faster code) — both upstreamed into LLVM and deployed in Google's production toolchain since around 2021. The repo ships the end-to-end stack: corpus extraction, training loop in TF-Agents, AOT-compiled-policy embedding back into `clang`/`opt`.

## Insight

The interesting bit isn't "ML can predict good inlining" — it's that MLGO solved the *deployment* problem. Production compilers can't tolerate a Python interpreter or a GPU at compile time, so MLGO trains a policy offline, AOT-compiles it via TF's `saved_model_cli` into a small statically-linked C++ artifact, and links it into LLVM. The resulting `clang` is one binary that takes no extra dependencies at use time but encodes a learned policy.

Where to reach for it:

- **You are building a custom LLVM-backed toolchain at scale** (Android, fleet rebuilds, datacenter binaries) and 1-3 % size or perf wins compound across millions of compilations. That's exactly Google's situation, and approximately nobody else's.
- **You are doing research on ML-for-systems / ML-for-compilers**. The repo is the cleanest open-source codebase for that workflow and pairs well with the [CompilerGym](https://github.com/facebookresearch/CompilerGym) RL environment (also Meta).

Where it isn't a fit:

- One-off projects: the cold path is heavy (corpus collection across thousands of TUs, hours of GPU training).
- Non-LLVM toolchains: MLGO is LLVM-IR-pass-shaped; porting to GCC or MSVC would be a research project.
- Reproducibility: each policy is tied to a corpus and an LLVM version; vendoring policies requires CI discipline.

The real impact has been intellectual: MLGO normalised the idea that compiler heuristics are a *learnable* surface, which has spawned follow-ups in Halide schedule search (Adams et al.), TVM auto-schedulers, learned cost models in MLIR, and the broader [[xla_accelerated_compiler|XLA]]/[[tiramisu|Tiramisu]] auto-tuning agenda.

## Similar / related topics

- **CompilerGym** (Meta) — Gym-shaped RL environment for compiler-optimisation research; complementary to MLGO.
- **Halide auto-schedulers** — Adams et al. 2019, learned cost models for image-processing kernel schedules.
- **TVM AutoTVM / Ansor / MetaSchedule** — ML-guided schedule search for tensor compilers.
- **AlphaDev** (DeepMind 2023) — RL discovers faster sorting routines at the assembly level; same family of ideas.
- **YaConvNet / GCC-MLGO** — research forks porting the idea to other compilers / passes.

## Internal links

<!-- reviewed -->
- [[xla_accelerated_compiler]] — Google's other ML-driven compiler effort, but at the tensor-graph level
- [[tiramisu]] — polyhedral compiler in the same broad "compile better with smarter search" space
- [[tiny_gpu]] — the hardware end of the same stack; useful contrast
- [[README|ml/compilers]] — section landing
- [[../../programming/rust/ml/cuda|programming/rust/ml/cuda]] — the Rust-CUDA side (different layer of the same stack)

## Keywords

`#mlgo` `#llvm` `#compiler-optimisation` `#reinforcement-learning` `#ml-for-systems` `#google`

## References / raw notes

### Google AI blog (announcement)

<https://ai.googleblog.com/2022/07/mlgo-machine-learning-framework-for.html>

> The question of how to compile faster and smaller code arose together with the birth of modern computers. Better code optimization can significantly reduce the operational cost of large datacenter applications. The size of compiled code matters the most to mobile and embedded systems or software deployed on secure-boot partitions, where the compiled binary must fit in tight code-size budgets. With advances in the field, the headroom has been heavily squeezed with increasingly complicated heuristics, impeding maintenance and further improvements.
>
> Recent research has shown that machine learning (ML) can unlock more opportunities in compiler optimization by replacing complicated heuristics with ML policies. However, adopting ML in general-purpose, industry-strength compilers remains a challenge.
>
> To address this, we introduce **MLGO: a Machine Learning Guided Compiler Optimizations Framework**, the first industrial-grade general framework for integrating ML techniques systematically in LLVM. MLGO uses reinforcement learning (RL) to train neural networks to make decisions that can replace heuristics in LLVM. We describe two MLGO optimizations for LLVM: 1) reducing code size with inlining; and 2) improving code performance with register allocation (regalloc). Both optimizations are available in the LLVM repository and have been deployed in production.

### Try it yourself

- Repo: <https://github.com/google/ml-compiler-opt>
- Demo (policy-gradient training of an inlining-for-size policy): <https://github.com/google/ml-compiler-opt/blob/main/docs/demo/demo.md>

### Paper (CGO 2021)

Trofin et al., "MLGO: a Machine Learning Guided Compiler Optimizations Framework" — <https://arxiv.org/abs/2101.04808>
