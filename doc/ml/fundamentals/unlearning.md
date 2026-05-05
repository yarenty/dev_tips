---
title: Unlearning
main_link: https://arxiv.org/abs/2502.20709?utm_source=tldrai
keywords: [unlearning, fundamentals, ml, knowledge, federated, fused, overwriting]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Unlearning

**Main link:** <https://arxiv.org/abs/2502.20709?utm_source=tldrai>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[ml/bigquery/links|links]] — Links _(score 15)_
- [[articles]] — Articles _(score 15)_
- [[public]] — Datasets _(score 15)_
- [[courses]] — top 5 2022 couses _(score 13)_
- [[graphnn]] — Graphnn _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#unlearning` `#fundamentals` `#ml` `#knowledge` `#federated` `#fused` `#overwriting`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->




Unlearning through Knowledge Overwriting: Reversible Federated Unlearning via Selective Sparse Adapter


https://arxiv.org/abs/2502.20709?utm_source=tldrai

Federated Learning is a promising paradigm for privacy-preserving collaborative model training. In practice, it is essential not only to continuously train the model to acquire new knowledge but also to guarantee old knowledge the right to be forgotten (i.e., federated unlearning), especially for privacy-sensitive information or harmful knowledge. However, current federated unlearning methods face several challenges, including indiscriminate unlearning of cross-client knowledge, irreversibility of unlearning, and significant unlearning costs. To this end, we propose a method named FUSED, which first identifies critical layers by analyzing each layer's sensitivity to knowledge and constructs sparse unlearning adapters for sensitive ones. Then, the adapters are trained without altering the original parameters, overwriting the unlearning knowledge with the remaining knowledge. This knowledge overwriting process enables FUSED to mitigate the effects of indiscriminate unlearning. Moreover, the introduction of independent adapters makes unlearning reversible and significantly reduces the unlearning costs. Finally, extensive experiments on three datasets across various unlearning scenarios demonstrate that FUSED's effectiveness is comparable to Retraining, surpassing all other baselines while greatly reducing unlearning costs.
