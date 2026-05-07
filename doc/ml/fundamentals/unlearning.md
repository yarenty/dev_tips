---
title: Machine unlearning
main_link: https://arxiv.org/abs/1912.03817
keywords: [unlearning, gdpr, federated-unlearning, sisa, right-to-be-forgotten]
status: reviewed
---

# Machine unlearning

**Main link:** <https://arxiv.org/abs/1912.03817>

## Summary

Machine unlearning is the problem of **removing the influence of specific training examples from
an already-trained model** without retraining from scratch — driven by GDPR Article 17
(right-to-be-forgotten), copyright takedowns, recovery from data poisoning, and evolving privacy
regulation (EU AI Act, US state laws). The canonical paper is **Bourtoule et al. 2019,
*Machine Unlearning*** (USENIX Security'21), which introduced the **SISA** (Sharded, Isolated,
Sliced, Aggregated) training framework: shard the data, train an independent sub-model per shard,
aggregate predictions; on a deletion request, only the affected shard's sub-model needs retraining.
The field splits into **exact unlearning** (provable equivalence to retraining-from-scratch on the
remaining data, e.g. SISA) and **approximate unlearning** (gradient-based "undo" updates, much
cheaper but with weaker guarantees).

## Insight

Three things to keep clear:

1. **Exact vs. approximate is a real distinction.** Exact unlearning gives you a defensible
   "this model behaves *as if* the data was never seen" story for regulators. Approximate
   methods (influence functions, gradient ascent on the forgotten examples, fine-tuning with
   negation losses) are dramatically cheaper but don't survive a determined membership-inference
   audit.
2. **Federated unlearning is its own subfield.** Federated training distributes the model across
   clients; unlearning has to coordinate across all of them without leaking which client requested
   the deletion. Recent work (FUSED — the FUSED paper at the original auto-stub link — Knowledge
   Overwriting via sparse adapters) targets this case with reversible adapter-only updates.
3. **The hardest case is LLMs.** Pretraining on terabytes of web text means deletion requests
   have a combinatorially-bad blast radius. Active research areas: localised "concept erasure"
   in attention heads, RLHF-style "forget this" fine-tuning, and approximate-unlearning benchmarks
   like TOFU and WMDP. None has a clean exact-unlearning story yet.

For most practical work in 2025: if regulatory exposure is real, design the training pipeline for
SISA-style sharding from the start — retrofitting unlearning onto a monolithically-trained model
is painful. If the model is small enough to retrain in hours, **just retrain** — the operational
simplicity beats any approximate-unlearning paper.

## Similar / related topics

- **Differential privacy** — adjacent: bound the influence of any single example *during training*, so the unlearning problem is smaller.
- **Data deletion / data lineage** — the systems-side companion problem (knowing *which* examples to forget when a user asks).
- **Model editing** (ROME, MEMIT) — surgical fact-overwriting in LLM weights; conceptually related but targets specific facts, not training examples.
- **Membership inference attacks** — the auditor's tool; how you measure whether unlearning actually worked.
- **Federated learning** — the distributed-training setting where federated unlearning lives.

## Internal links
<!-- reviewed -->
- [[../llm/README|llm]] — sibling section; LLM unlearning is the hardest open problem.
- [[../finetuning/README|finetuning]] — adjacent: many approximate-unlearning techniques are negation-fine-tuning.
- [[../frameworks/README|frameworks]] — what SISA-style training pipelines build on.
- [[classification]], [[pattern_learning]] — sibling fundamentals topics.

## Keywords

`#unlearning` `#gdpr` `#federated-unlearning` `#sisa` `#right-to-be-forgotten`

## References / raw notes

- Foundational: [Bourtoule et al. 2021, *Machine Unlearning*](https://arxiv.org/abs/1912.03817) (USENIX Security'21) — introduces SISA.
- Federated unlearning: [*Unlearning through Knowledge Overwriting: Reversible Federated Unlearning via Selective Sparse Adapter (FUSED)*](https://arxiv.org/abs/2502.20709). Federated learning method that identifies critical layers by analysing per-layer sensitivity, builds sparse unlearning adapters for them, and overwrites unlearning knowledge with the remaining knowledge — making unlearning reversible and much cheaper than retraining.
- Survey: [Nguyen et al. 2022, *A Survey of Machine Unlearning*](https://arxiv.org/abs/2209.02299).
- LLM unlearning benchmarks:
  - [TOFU (Maini et al. 2024)](https://arxiv.org/abs/2401.06121).
  - [WMDP (Li et al. 2024)](https://arxiv.org/abs/2403.03218).
- Regulatory context: [GDPR Article 17 — Right to erasure](https://gdpr-info.eu/art-17-gdpr/), [EU AI Act](https://artificialintelligenceact.eu/).
