---
title: Kevin-32B — Cognition's CUDA-kernel-writing model
main_link: https://cognition.ai/blog/kevin-32b
keywords: [kevin, kevin-32b, cognition-ai, cuda, kernel-generation, multi-turn-rl, kernelbench, llm-codegen]
status: reviewed
---

# Kevin-32B — Cognition's CUDA-kernel-writing model

**Main link:** <https://cognition.ai/blog/kevin-32b>

## Summary

Kevin-32B (= "**K**ernel D**evin**", from the makers of Devin) is a 32-billion-parameter open-weight model from Cognition AI, fine-tuned with **multi-turn reinforcement learning** specifically for writing CUDA kernels. On KernelBench (the standard CUDA-kernel-generation benchmark introduced by Stanford in 2024), it reportedly outperforms frontier reasoning models like DeepSeek-R1, OpenAI o1/o3-mini, and Claude on the kernel-generation task — with the headline finding being that **multi-turn RL training (model proposes → compiles → tests → revises) produces materially better self-refinement than single-turn training**, exactly the regime real GPU-kernel work lives in.

## Insight

Why this matters beyond a benchmark headline: writing a correct, fast CUDA kernel is a **multi-shot edit-compile-profile loop** by nature — you write something, `nvcc` complains, you fix it, `cuda-memcheck` reports a race, you fix that, `nsys` shows you the kernel is memory-bound, you re-tile, etc. Single-turn LLM evals (one prompt → one answer) systematically underestimate model capability on this kind of work. Cognition's contribution is showing — quite cleanly — that if you train the model in the same loop it will be deployed in (multi-turn with verifier feedback), the gap closes dramatically.

Where this fits in the broader landscape:

- It joins a growing pile of **agentic-coding RL** work: SWE-RL (Cognition / SWE-Bench), Goose (Block), DeepSeek-Coder-V2's RL stage, AlphaCode 2's iterative search.
- KernelBench itself ([Sakana AI / Stanford repo](https://github.com/ScalingIntelligence/KernelBench)) has become the standard for "can LLM write a useful CUDA kernel?", with companion benchmarks like KernelBot.
- The base model is open-weight (released on Hugging Face under cognition-ai/Kevin-32B) which makes it useful for local experiments, but it's task-specific — don't expect it to beat a generalist on non-kernel tasks.

Where it isn't a fit:

- Production CUDA kernel-writing in 2025 is still mostly humans + libraries (cuBLAS / cuDNN / CUTLASS / Triton). LLM-written kernels are a research / autotune-augmenter angle, not a replacement for a real CUDA engineer.
- The "outperforms frontier models" claim is on KernelBench specifically — apples-to-oranges comparisons against o3 or Claude on general code are not what's being shown.
- Hardware-specific optimisation knowledge (Hopper async copies, B200 TMA, FP8 quantisation gotchas) is a moving target; any model trained today rots quickly.

The bigger trend Kevin sits inside: **LLMs that write and improve kernels in a loop with a verifier**, alongside DeepMind's AlphaTensor (matmul algorithms), AlphaDev (sorting / hash routines at the assembly level), and Sakana's CUDA-Engineer agent. The autotuning-by-LLM regime appears to actually work for narrow, well-specified, easily-verified domains like kernel generation.

## Similar / related topics

- **KernelBench** — Stanford/Sakana benchmark for LLM-written CUDA kernels; the eval Kevin targets.
- **Sakana AI's CUDA Engineer / AI Scientist** — agentic LLM systems for kernel and research-paper writing.
- **DeepSeek-R1 / o1 / o3 / Claude 3.5/3.7 / Qwen-Coder** — the frontier reasoning models Kevin is benchmarked against.
- **Triton (OpenAI)** — the actual production path most non-CUDA-experts take to write GPU kernels.
- **CUTLASS / cuBLAS / cuDNN** — what production kernels are usually built from rather than written from scratch.
- **AlphaTensor / AlphaDev** — DeepMind's RL-finds-better-algorithms work, same family of ideas.

## Internal links

- [[README|cuda]] — broader CUDA hub (toolchain, libraries, ecosystem)
- [[../README|ml/compilers]] — broader ML-compilers section
- [[../xla_accelerated_compiler|xla_accelerated_compiler]] — what compiles to CUDA when you don't write the kernels
- [[../tiny_gpu]] — for understanding the GPU you're writing kernels for
- [[../../../programming/rust/ml/cuda|programming/rust/ml/cuda]] — Rust-side CUDA bindings
- [[../../llm/README|ml/llm]] — broader LLM section (Kevin is an LLM specialised by RL)

## Keywords

`#kevin` `#kevin-32b` `#cognition-ai` `#cuda` `#kernel-generation` `#multi-turn-rl` `#kernelbench` `#llm-codegen`

## References / raw notes

### Cognition blog post (announcement)

<https://cognition.ai/blog/kevin-32b>

> **Kevin-32B: Multi-Turn RL for Writing CUDA Kernels.** Kevin-32B = K(ernel D)evin, outperforms frontier reasoning models on kernel generation. Moreover, our results show that multi-turn training makes the model more effective at self-refinement compared to single-turn training.

### Hugging Face model card

- Repo: <https://huggingface.co/cognition-ai/Kevin-32B>
- README / details: <https://huggingface.co/cognition-ai/Kevin-32B/blob/main/README.md>

### Benchmark context

- KernelBench (Stanford / ScalingIntelligence): <https://github.com/ScalingIntelligence/KernelBench>
- Sakana AI CUDA Engineer (related agentic kernel-writing system): <https://sakana.ai/ai-cuda-engineer/>

### Cognition AI

- Cognition is the team behind Devin (the "AI software engineer"): <https://cognition.ai/>
