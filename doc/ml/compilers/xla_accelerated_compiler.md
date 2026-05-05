---
title: XLA
main_link: https://github.com/openxla/xla
keywords: [xla-accelerated-compiler, compilers, tensorflow, gpus, cpus, linear, frameworks]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# XLA

**Main link:** <https://github.com/openxla/xla>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tiny_gpu]] — Tiny - GPU _(score 28.9)_
- [[tiramisu]] — Tiramisu _(score 22.4)_
- [[mlgo_llvm]] — MLGO _(score 22.4)_
- [[vortex]] — Vortex (2024-10-17) _(score 12.9)_
- [[rust_ml]] — BAD ONES: _(score 12.3)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#xla-accelerated-compiler` `#compilers` `#ml` `#tensorflow` `#gpus` `#cpus` `#pytorch`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# XLA

https://github.com/openxla/xla

**XLA (Accelerated Linear Algebra)** is an open-source machine learning (ML) compiler for GPUs, CPUs, and ML accelerators.

The XLA compiler takes models from popular ML frameworks such as PyTorch, TensorFlow, and JAX, and optimizes them for high-performance execution across different hardware platforms including GPUs, CPUs, and ML accelerators.

## Get started
If you want to use XLA to compile your ML project, refer to the corresponding documentation for your ML framework:

- PyTorch
- TensorFlow
- JAX



Clone this repository:
```shell
git clone https://github.com/openxla/xla && cd xla
```

We recommend using a suitable docker container to build/test XLA, such as TensorFlow's docker container:

```shell
docker run --name xla -w /xla -it -d --rm -v $PWD:/xla tensorflow/build:latest-python3.9 bash
```

Run an end to end test using an example StableHLO module:

```shell
docker exec xla ./configure

docker exec xla bazel test xla/examples/axpy:stablehlo_compile_test --nocheck_visibility --test_output=all
```

This will take quite a while your first time because it must build the entire stack, including MLIR, StableHLO, XLA, and more.
