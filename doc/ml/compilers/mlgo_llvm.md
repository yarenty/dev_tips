---
title: MLGO
main_link: https://github.com/google/ml-compiler-opt
keywords: [mlgo-llvm, compilers, ml, llvm, learning, machine]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# MLGO

**Main link:** <https://github.com/google/ml-compiler-opt>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[mlcube]] — MLCube _(score 27.0)_
- [[mindsdb]] — MindsDB _(score 27.0)_
- [[ml/bigquery/links|links]] — Links _(score 14.4)_
- [[co_scientist]] — Google _(score 14.4)_
- [[articles]] — Articles _(score 14.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#mlgo-llvm` `#compilers` `#ml` `#compiler` `#llvm` `#learning` `#machine`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# MLGO

A Machine Learning Framework for Compiler Optimization


https://ai.googleblog.com/2022/07/mlgo-machine-learning-framework-for.html



The question of how to compile faster and smaller code arose together with the birth of modem computers. Better code optimization can significantly reduce the operational cost of large datacenter applications. The size of compiled code matters the most to mobile and embedded systems or software deployed on secure boot partitions, where the compiled binary must fit in tight code size budgets. With advances in the field, the headroom has been heavily squeezed with increasingly complicated heuristics, impeding maintenance and further improvements.

Recent research has shown that machine learning (ML) can unlock more opportunities in compiler optimization by replacing complicated heuristics with ML policies. However, adopting ML in general-purpose, industry-strength compilers remains a challenge.

To address this, we introduce “MLGO: a Machine Learning Guided Compiler Optimizations Framework”, the first industrial-grade general framework for integrating ML techniques systematically in LLVM (an open-source industrial compiler infrastructure that is ubiquitous for building mission-critical, high-performance software). MLGO uses reinforcement learning (RL) to train neural networks to make decisions that can replace heuristics in LLVM. We describe two MLGO optimizations for LLVM: 1) reducing code size with inlining; and 2) improving code performance with register allocation (regalloc). Both optimizations are available in the LLVM repository, and have been deployed in production.


Try it Yourself
Check out the open-sourced end-to-end data collection and training solution on github and a demo that uses policy gradient to train an inlining-for-size policy.

https://github.com/google/ml-compiler-opt

https://github.com/google/ml-compiler-opt/blob/main/docs/demo/demo.md
