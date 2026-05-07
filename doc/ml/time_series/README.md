---
title: Time series — forecasting, anomaly detection, modern architectures
keywords: [time-series, forecasting, anomaly-detection, transformer, kan, dlinear]
status: reviewed
---

# Time series — forecasting, anomaly detection, modern architectures

Notes on time-series forecasting (statistical, ML, DL) and the libraries that implement it. The angle is practitioner-focused — what to reach for, what to avoid, what the canonical baselines are — with explicit cross-links to the sibling section [[../../db/timeseries/README|db/timeseries]] (InfluxDB, QuestDB, GreptimeDB), which covers the *substrate* on which TS data lives.

## Reading order

1. **Start with [[forecasting]]** for the ecosystem map (statistical / ML / DL families, library landscape, M-competition lessons).
2. **Read [[linear_regression]]** for the boring baseline you should always run first.
3. **Read [[missing_data]]** for the pre-processing gotchas everyone hits.
4. **Then pick a depth dive:**
   - [[time_series_transformer]] for the Transformer-architecture lineage (Informer → Autoformer → FEDformer → PatchTST → iTransformer) plus the foundation-model wave (TimesFM, Chronos, Lag-Llama, Moirai), with the honest DLinear caveat.
   - [[kan]] for KANs as MLP-replacements inside TS models.
   - [[time_sieve]] for the wavelet + information-bottleneck non-Transformer trend.
5. **Then read [[ml/time_series/papers|papers]]** for the broader research reading list (M-competitions, anomaly-detection, foundation models).
6. **Browse [[ml/time_series/tutorials|tutorials]]** for the curated learning resources (Kaggle Learn, Hyndman *FPP3*, Nixtla docs, darts user guide).

## Decision shortcut

| Situation | Reach for |
|-----------|-----------|
| Single short series, regular seasonality | ARIMA / SARIMA / ETS via `statsmodels` |
| Many short series, exogenous features | LightGBM/XGBoost on lag features (`mlforecast`) |
| Business forecasting with holidays | Prophet (Meta) |
| Long horizon, large multivariate dataset | PatchTST or iTransformer (`neuralforecast`) |
| Cold start, no training data | Foundation models — TimesFM / Chronos / Moirai (zero-shot) |
| Strong frequency-band structure | TimeSieve (wavelet) or FEDformer (Fourier) |
| Anomaly detection on subsequences | Matrix Profile via `stumpy` |
| Just want one library that does everything | `darts` (broadest API) or Nixtla family (most active) |

## Articles

- [[forecasting]] — methods, libraries, and trade-offs (the section's main map)
- [[time_series_transformer]] — Transformer architectures, foundation models, DLinear caveat
- [[kan]] — Kolmogorov-Arnold Networks for TS forecasting
- [[time_sieve]] — wavelet + information-bottleneck (TimeSieve)
- [[linear_regression]] — the canonical baseline with engineered features
- [[missing_data]] — imputation strategies (forward-fill, interpolation, Kalman, MICE)
- [[ml/time_series/papers|papers]] — research reading list (forecasting + anomaly detection)
- [[ml/time_series/tutorials|tutorials]] — courses, books, library guides

## See also

- [[../fundamentals/README|ml/fundamentals]] — foundational ML topics (`[[../fundamentals/kan|fundamentals/kan]]` for the canonical KAN article — note ambiguity with `[[kan|time_series/kan]]` here)
- [[../frameworks/README|ml/frameworks]] — Mindsdb, H2O AutoML, BigQuery ML, etc.
- [[../tools/README|ml/tools]] — broader ML tooling
- [[../../db/timeseries/README|db/timeseries]] — TS databases (InfluxDB, QuestDB, GreptimeDB, Druid)
- [[../../trading/README|trading]] — applied forecasting in algo-trading

## Keywords

`#time-series` `#forecasting` `#anomaly-detection` `#arima` `#prophet` `#transformer` `#dlinear` `#patchtst` `#itransformer` `#timesfm` `#chronos` `#kan`
