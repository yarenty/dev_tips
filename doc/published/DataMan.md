---
title: DataMan — LLM-driven pre-training data selection (paper summary)
main_link: https://arxiv.org/abs/2502.19178
keywords: [dataman, paper, llm, pre-training, data-selection, data-quality, alibaba]
status: reviewed
---

# DataMan — LLM-driven pre-training data selection (paper summary)

**Main link:** <https://arxiv.org/abs/2502.19178>

## Summary

Paper summary of *DataMan: Data Manager for Pre-training Large Language Models* (Peng et al., Alibaba / Tongyi, Feb 2025 — arXiv 2502.19178). DataMan tackles the *which subset of the pre-training corpus should I actually train on?* question: it annotates a 447B-token corpus (a derivative of SlimPajama called **DataPajama**) with **14 LLM-self-identified quality criteria + 15 domain types**, fine-tunes a small (1.5B Qwen2-based) **DataMan model** to do the annotation cheaply, then uses top-k sampling per criterion + per domain to pick a high-quality, diverse subset. Models pre-trained on the DataMan-curated subset beat DSIR / perplexity-filtering / Qurating baselines on perplexity, in-context learning (ICL), and instruction-following (78.5% AlpacaFarm win rate against the SOTA baseline).

## Insight

The interesting trick is **"reverse thinking"**: instead of guessing what makes a document "good for pre-training", the authors prompted a strong LLM to look at documents at the top vs bottom of the perplexity distribution and *self-identify* which criteria correlate with its own performance. They got 14 criteria (Accuracy, Coherence, Language Consistency, Semantic Density, Knowledge Novelty, Topic Focus, Creativity, Professionalism, Grammatical Diversity, Structural Standardization, Originality, Sensitivity, Educational Value, Style Consistency) plus an Overall Score. Most of these don't correlate with raw perplexity — i.e. **DataMan finds quality signal that perplexity-filtering misses**.

When this matters:
- **You're pre-training (or continued-pre-training) a base model.** This is the modern data-selection state of the art; the alternative is to use Qurating, FineWeb's heuristics, or just "trust the source".
- **You're doing domain mixing** (legal + medical + code). DataMan's 86% domain-recognition accuracy makes the per-domain top-k recipe work cleanly.
- **You care about ICL / instruction-following downstream**, not just next-token loss — these are the gains the paper actually demonstrates.

When to skip:
- You're fine-tuning a chat/instruct model — use [[../ml/finetuning/README|finetuning]] instead; DataMan is for the pre-training stage.
- You're not training from scratch — just picking a strong base model (e.g. [[../ml/llm/models/llama|llama]] or [[../ml/llm/models/deepseek|deepseek]]) gets you the benefit indirectly.
- Honest caveat: results are on SlimPajama only, the 1.3B model size is small, and the rating distribution is concentrated at 4-5 (so the "quality signal" is partly the LLM's own bias).

## Similar / related topics

- **Qurating** (Wettig et al., 2024) — an earlier LLM-rated-quality data-selection method; the SOTA baseline DataMan beats.
- **DSIR** (Xie et al., 2023) — Data Selection with Importance Resampling; the "target a domain via n-gram similarity" approach.
- **Perplexity filtering** — the long-standing baseline (e.g. CCNet, Pile preprocessing); DataMan shows it under-performs even random uniform sampling.
- **FineWeb / FineWeb-Edu** (HuggingFace, 2024) — production-scale Common-Crawl curation; uses a similar LLM-as-judge classifier for educational-value filtering.
- **The Pile / RedPajama / SlimPajama / Dolma** — the canonical open pre-training corpora DataMan is built on top of.
- **Self-rewarding LLMs** (Yuan et al., 2024) — same "use the LLM to grade itself" pattern, applied to alignment instead of pre-training.

## Internal links

- [[../ml/data/public|public datasets]] — the broader pre-training-corpora landscape (Common Crawl / FineWeb / RedPajama / Dolma / The Pile) DataMan is curating from.
- [[../ml/llm/models/olmo|olmo]] — Allen AI's "fully open" base model whose Dolma corpus is the natural target for this kind of curation work.
- [[../ml/llm/models/llama|llama]] — the canonical pre-trained-from-curated-data base model.
- [[../ml/llm/models/deepseek|deepseek]] — another data-quality-obsessed pre-training story (DeepSeek-V3's data curation is a common reference point).
- [[../db/quality/data_quality|data_quality]] — the broader data-quality-as-a-discipline angle (DAMA dimensions, DQ frameworks).
- [[../db/quality/data_quality_problems_in_AI/README|data_quality_problems_in_AI]] — ML-side failure modes (label noise, drift, contamination) DataMan complements.
- [[../db/quality/data_curation/README|data_curation]] — the operational side (Argilla / Snorkel / NeMo Curator / Datatrove) where DataMan-style classifiers get deployed.
- [[../ml/llm/inspection/leaderboards|leaderboards]] — where the downstream wins (or losses) ultimately get measured.

## Keywords

`#paper` `#llm` `#pre-training` `#data-selection` `#data-quality` `#alibaba` `#tongyi`

## References / raw notes

- **Paper:** Peng, R. et al. *DataMan: Data Manager for Pre-training Large Language Models.* arXiv:2502.19178 (Feb 2025). <https://arxiv.org/abs/2502.19178>
- **Affiliated team:** Alibaba Tongyi Lab.
- **Code / models:** released on HuggingFace under the DataMan tag.

### Distilled paper notes

**Contributions**
- A 447B-token corpus (DataPajama, derived from SlimPajama) annotated with 14 quality criteria + 15 domain types.
- A 1.5B-parameter DataMan model (fine-tuned from Qwen2-1.5B) that does the annotation cheaply at scale.
- A pre-training data-selection recipe (top-k per criterion across source/domain distribution) that beats DSIR / perplexity filtering / Qurating on perplexity, ICL, and instruction-following.

**Annotation approach ("reverse thinking")**
- Prompt a Super LLM (the paper uses a strong frontier model) with documents at the top and bottom of perplexity to self-identify criteria that correlate with its own performance.
- 14 criteria emerge: Accuracy / Coherence / Language Consistency / Semantic Density / Knowledge Novelty / Topic Focus / Creativity / Professionalism / Grammatical Diversity / Structural Standardization / Originality / Sensitivity / Educational Value / Style Consistency.
- Plus one Overall Score and one Domain label (15 categories).
- DataMan model trained with text-generation loss on the Super-LLM-rated dataset; ~80% average test accuracy, 81.3% on Overall Score, 86% on domain recognition.
- **Pointwise rating chosen over pairwise** — because pairwise breaks down when quality differences are marginal, and pointwise rating error is bounded by the model's training loss.

**Pre-training data selection**
- Apply the DataMan model to annotate every document in DataPajama.
- Top-k sampling without replacement per criterion across source + domain distribution maximises representativeness while preserving diversity.
- Overall Score ratings used to weight the language-modelling loss (data-reward / reward-weighted regression).

**Experiments**
- Train language model from scratch on 447B-token DataPajama for pre-training.
- Compared against: Uniform sampling, DSIR, Perplexity filtering, Qurating sampling, DataMan sampling.
- Metrics: perplexity, in-context learning (ICL), instruction-following (AlpacaFarm).

**Headline results**
- DSIR and perplexity filtering perform *worse than uniform sampling* — the surprise of the paper.
- Qurating helps, but a *mix* of unclear criteria fails (no complementarity).
- DataMan beats Qurating; Overall Score is the best single criterion.
- Sample-with-DataMan reaches 78.5% AlpacaFarm win rate vs SOTA baseline.
- Domain recognition helps domain-specific ICL (the per-domain top-k matters).

**Analysis**
- Quality ratings concentrate at 4-5 — most documents are "high" quality on most criteria (this is partly LLM-judge-bias).
- Knowledge Novelty and Creativity get the lowest average scores — these are the genuinely scarce qualities.
- Most criteria do *not* correlate with perplexity (except Structural Standardization, Professionalism, Creativity) — i.e. DataMan finds signal perplexity misses.

**Limitations the paper acknowledges**
- LLM-judge bias is inherited; needs more-diverse documents to characterise.
- SlimPajama-only experiments limit external validity.
- 1.3B model size is small; results may not transfer to 70B+.
- Compute budget significantly constrained the study.

