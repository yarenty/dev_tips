---
title: TimeSieve — wavelet + information-bottleneck for time series
main_link: https://arxiv.org/abs/2406.05036
keywords: [timesieve, time-series, forecasting, wavelet, information-bottleneck, dlinear, non-transformer]
status: reviewed
---

# TimeSieve — wavelet + information-bottleneck for time series

**Main link:** <https://arxiv.org/abs/2406.05036> (Feng et al., *TimeSieve: Extracting Temporal Dynamics through Information Bottlenecks*, 2024)

## Summary

TimeSieve is a 2024 non-Transformer architecture for time-series forecasting. It runs the input through a **discrete wavelet transform** to separate frequency components, then applies an **information-bottleneck**-based filtering layer to keep only the predictive parts of each frequency band, and finally inverse-transforms back. The pitch is competitive accuracy on the standard ETT/Weather/Electricity/Traffic benchmarks with a small parameter count and no attention. Code is on GitHub at `xll0328/TimeSieve`.

## Insight

- **Where it sits.** TimeSieve is part of a wider 2022-2024 trend pushing back on Transformers for TS forecasting. Other members of this "non-Transformer alternatives" cohort: **DLinear/NLinear** (Zeng 2022 — a literal linear layer matches Informer/Autoformer/FEDformer), **RLinear** (reversible normalisation + linear), **TiDE** (Google, MLP-based), **TSMixer** (Google, all-MLP), and **Mamba**-based variants. The shared message: attention is not the magic ingredient in TS that it is in NLP.
- **The wavelet angle is the differentiator.** Most TS-DL works on the time domain or uses a Fourier basis (FEDformer). Wavelets give multi-resolution decomposition — coarse trends *and* localised events — which fits TS better than Fourier when the signal has burst-like behaviour. The information-bottleneck filter then prunes uninformative wavelet coefficients.
- **Reach for it when:** you want a strong non-attention baseline to compare against PatchTST/iTransformer; you have signals with distinct frequency bands (sensor data, audio-like signals, EEG); you care about parameter count.
- **Caveats.** Single-paper architecture, niche reproductions only. For production work prefer something with library-level support: PatchTST/iTransformer/N-BEATS/NHiTS via `darts` or `neuralforecast`. Treat TimeSieve as a research lead, not a default.
- **What "information bottleneck" means here.** The IB principle (Tishby) trades off compression of the input against retention of label-predictive information; in TimeSieve it's used to learn a sparse mask over wavelet coefficients.

## Similar / related topics

- DLinear / NLinear — Zeng et al.'s "are Transformers effective?" tiny linear baselines
- TiDE — Google's MLP-based long-horizon forecaster
- TSMixer — Google's all-MLP architecture for multivariate TS
- FEDformer — Fourier-domain decomposition Transformer (closest sibling architecturally)
- N-BEATS / NHiTS — MLP-residual stacks; the "boring DL" baselines that stay competitive

## Internal links
<!-- reviewed -->

- [[forecasting]]
- [[time_series_transformer]]
- [[kan]]
- [[ml/time_series/papers|papers]]
- [[README]]

## Keywords

`#timesieve` `#time-series` `#forecasting` `#wavelet` `#information-bottleneck` `#dlinear` `#non-transformer` `#deep-learning`

## References / raw notes

- Paper: Feng et al., *TimeSieve: Extracting Temporal Dynamics through Information Bottlenecks* (2024) — <https://arxiv.org/abs/2406.05036>
- Code: <https://github.com/xll0328/TimeSieve>
- Architecture diagram: <https://github.com/xll0328/TimeSieve/raw/main/model.png>

Adjacent papers worth reading alongside:

- Zeng et al., *Are Transformers Effective for Time Series Forecasting?* (AAAI 2023, the **DLinear** paper) — <https://arxiv.org/abs/2205.13504>
- Das et al., *TiDE: Time-series Dense Encoder* (Google 2023) — <https://arxiv.org/abs/2304.08424>
- Chen et al., *TSMixer: An All-MLP Architecture for Time Series Forecasting* (Google 2023) — <https://arxiv.org/abs/2303.06053>
- Tishby & Zaslavsky, *Deep Learning and the Information Bottleneck Principle* (2015) — IB foundations.
