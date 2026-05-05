---
title: Fed-SB
main_link: https://github.com/CERT-Lab/fed-sb
keywords: [fedsb, finetuning, ml, fed, lora, federated, communication]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Fed-SB

**Main link:** <https://github.com/CERT-Lab/fed-sb>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[task]] — Task _(score 15)_
- [[ml/bigquery/links|links]] — Links _(score 15)_
- [[articles]] — Articles _(score 15)_
- [[agentic_ai_memory]] — Task _(score 15)_
- [[unsloth]] — unsloth _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#fedsb` `#finetuning` `#ml` `#fed` `#lora` `#federated` `#communication`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Fed-SB

paper: https://arxiv.org/pdf/2502.15436v1

https://github.com/CERT-Lab/fed-sb

Fed-SB: A Silver Bullet for Extreme Communication Efficiency and Performance in (Private) Federated LoRA Fine-Tuning

## Introduction
Low-Rank Adaptation (LoRA) has become ubiquitous for efficiently fine-tuning foundation models. However, federated fine-tuning of LoRA remains challenging due to non-optimal updates arising from traditional federated averaging of individual adapters. Existing solutions either incur prohibitively high communication cost that scales linearly with the number of clients or suffer from performance degradation due to limited expressivity. We introduce Federated Silver Bullet (Fed-SB), a novel approach for federated fine-tuning of LLMs using LoRA-SB, a recently proposed low-rank adaptation method. LoRA-SB learns a rxr matrix between B and A, while keeping other components fixed. Direct averaging of R guarantees exact updates, substantially reducing communication cost, which remains independent of the number of clients, and enables scalability. Fed-SB achieves state-of-the-art performance across commonsense reasoning, arithmetic reasoning, and language inference tasks while reducing communication costs by up to 230x. In private settings, Fed-SB further improves performance by (1) reducing trainable parameters, thereby lowering the noise required for differential privacy, and (2) avoiding noise amplification introduced by other methods. Overall, Fed-SB establishes a new Pareto frontier in performance vs. communication cost, offering an efficient and scalable solution for both private and non-private federated fine-tuning.


![](https://github.com/CERT-Lab/fed-sb/raw/main/assets/results.png)
