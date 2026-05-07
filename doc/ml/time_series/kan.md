---
title: KANs for time-series forecasting
main_link: https://arxiv.org/abs/2405.08790
keywords: [kan, kolmogorov-arnold, time-series, forecasting, mlp, n-beats, nhits, tsmixer]
status: reviewed
---

# KANs for time-series forecasting

**Main link:** <https://arxiv.org/abs/2405.08790> (*KANs for Time Series Forecasting*, Vaca-Rubio et al., 2024)

## Summary

Kolmogorov-Arnold Networks (KANs) replace the MLP's fixed activations + learned weights with **learnable univariate functions on edges** (typically B-spline parametrised). Several 2024 papers tried KANs as drop-in MLP replacements inside time-series forecasters — KAN-only models, plus KAN-backboned variants of N-BEATS, TSMixer and similar. The empirical case is mixed: on standard ETT/Electricity/Weather benchmarks, KAN forecasters are roughly competitive with their MLP counterparts at lower parameter counts but rarely a decisive win, and they cost more per-parameter to train.

## Insight

- **Why people care for TS specifically.** TS-DL is dominated by MLP backbones (N-BEATS, NHiTS, TSMixer, DLinear, RLinear). If KANs really are "MLPs but better with fewer parameters", they should swap in cleanly — and they roughly do, which is why there was a small flood of "X-KAN" variants in mid-2024.
- **Honest reality check.** The original KAN paper's strongest claims (interpretability, scientific discovery) don't carry over directly to noisy financial / sensor / energy forecasting tasks. On the standard long-horizon TS benchmarks, **DLinear and PatchTST already beat the Transformer family** with tiny linear models — so the "KAN beats MLP" bar moves: it has to beat a one-layer linear projection too, which is hard.
- **Reach for it when:** you specifically want learned-activation interpretability (e.g. extracting symbolic forms via `pykan`'s symbolic regression), or you're researching architecture choices and want a non-MLP comparison point. Don't reach for it for production forecasting in 2025 — pick from the Nixtla/darts library and benchmark.
- **Implementation note.** First-gen KAN (`pykan`) is slow because of B-spline evaluation. **FastKAN, EfficientKAN, Wav-KAN** rewrites are 5-100× faster and what you'd actually use.

## Similar / related topics

- [[../fundamentals/kan|fundamentals/kan]] — the canonical KAN article (architecture, the Kolmogorov-Arnold representation theorem, FastKAN/EfficientKAN/Wav-KAN follow-ups)
- [[forecasting]] — broader Python forecasting ecosystem this fits into
- [[time_series_transformer]] — the other "fancy DL" lineage for TS
- [[time_sieve]] — non-MLP, non-Transformer TS architecture (wavelet + information bottleneck)

## Internal links
<!-- reviewed -->

- [[../fundamentals/kan|fundamentals/kan]] — canonical KAN article
- [[forecasting]]
- [[time_series_transformer]]
- [[time_sieve]]
- [[ml/time_series/papers|papers]]
- [[README]]

## Keywords

`#kan` `#kolmogorov-arnold` `#time-series` `#forecasting` `#mlp` `#n-beats` `#nhits` `#tsmixer` `#deep-learning`

## References / raw notes

- Vaca-Rubio, Blanco, Pereira, Caus, *KANs for Time Series Forecasting* (May 2024) — <https://arxiv.org/abs/2405.08790>
- Liu et al., *KAN: Kolmogorov-Arnold Networks* (Apr 2024) — <https://arxiv.org/pdf/2404.19756>
- Marco Peixeiro's TDS walkthrough — <https://towardsdatascience.com/kolmogorov-arnold-networks-kans-for-time-series-forecasting-9d49318c3172>
- N-BEATS paper — <https://arxiv.org/abs/1905.10437>
- NHiTS paper — <https://arxiv.org/pdf/2201.12886>
- TSMixer (Google) — <https://towardsdatascience.com/tsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb>

KAN replaces the MLP's `Linear(W) → fixed-σ` block with a sum of learnable univariate B-spline functions on each edge. The Kolmogorov-Arnold representation theorem guarantees that any continuous multivariate function can be written as a finite composition of continuous univariate functions plus addition — KAN parameterises that.

For TS, a typical experiment swaps the MLP block inside N-BEATS or TSMixer with a KAN block, holds everything else fixed (windowing, scaling, loss), and reports MAE/MSE on ETTh1/h2/m1/m2, Electricity, Weather, Traffic. Result is usually "competitive at fewer parameters", which is interesting but not transformative.
