---
title: Transformers for time series — architectures, lineage, and the DLinear caveat
main_link: https://github.com/allen-chiang/Time-Series-Transformer
keywords: [transformer, time-series, informer, autoformer, fedformer, patchtst, itransformer, timesfm, chronos, dlinear]
status: reviewed
---

# Transformers for time series — architectures, lineage, and the DLinear caveat

**Main link:** <https://github.com/allen-chiang/Time-Series-Transformer> (Allen Chiang, *Time-Series-Transform* — the small Python package the original notes documented)

## Summary

Two distinct things share the name "Time Series Transformer". (1) Allen Chiang's `Time-Series-Transform` Python package — a data-engineering library for shaping multi-dimensional TS for ML pipelines (lag/lead windowing, technical-indicator wiring, multi-category aggregation, plotting). (2) The much larger **architecture lineage** that adapts the 2017 attention-based Transformer to forecasting — Informer (2021), Autoformer (2021), FEDformer (2022), PatchTST (2023), iTransformer (2024) — and the 2024 wave of TS foundation models (TimesFM, Chronos, Lag-Llama, Moirai). This article covers both, with the honest empirical caveat that pure-attention models in TS forecasting are *less* dominant than they are in NLP.

## Insight

### The architecture lineage (what people usually mean)

| Year | Model | What's new |
|------|-------|------------|
| 2017 | vanilla Transformer | self-attention, no recurrence — but O(n²) memory bites for long horizons |
| 2021 | **Informer** (AAAI) | ProbSparse attention, distilling, generative-style decoder for long-horizon |
| 2021 | **Autoformer** (NeurIPS) | series decomposition + auto-correlation instead of attention |
| 2022 | **FEDformer** (ICML) | frequency-enhanced decomposition (Fourier/wavelet) + mixture-of-experts |
| 2022 | **DLinear / NLinear** (Zeng et al.) | "are Transformers effective?" — a tiny linear layer matches/beats them all |
| 2023 | **PatchTST** (ICLR) | tokenise patches of the series, channel-independence — the first cleanly-better Transformer |
| 2024 | **iTransformer** (ICLR) | invert dimensions: each *variate* is a token, not each timestep |
| 2024 | **TimesFM** (Google) | 200M decoder-only, pretrained on 100B time-points, zero-shot |
| 2024 | **Chronos** (Amazon) | tokenise values into a vocabulary, fine-tune T5/GPT family |
| 2024 | **Lag-Llama** | Llama-style decoder for univariate forecasting |
| 2024 | **Moirai** (Salesforce) | multi-variate, multi-frequency foundation model |

### The honest caveat

Zeng et al.'s *Are Transformers Effective for Time Series Forecasting?* (AAAI 2023) showed that **DLinear** — literally `decompose into trend + seasonal, fit a linear layer to each` — matches or beats Informer/Autoformer/FEDformer on the standard benchmarks (ETT, Weather, Electricity, Traffic, ILI). PatchTST and iTransformer recovered some ground for the Transformer family, but the lesson stuck: in TS, attention isn't the magic ingredient it is in NLP. Series are short, autocorrelated, and lack the rich token structure that makes attention shine.

### When to reach for the Transformer family

- **Large dataset, long horizon, many series.** PatchTST / iTransformer scale better than RNNs and (sometimes) than DLinear once you have lots of data.
- **Cold-start, no training data.** Foundation models — TimesFM, Chronos — give surprisingly good zero-shot forecasts. Useful baseline in production before you have history.
- **Complex multivariate dependencies.** iTransformer's "variates as tokens" view captures cross-series structure cleanly.

### When NOT to reach for them

- **Few series, short history.** ARIMA / ETS / Prophet wins.
- **Tabular-ish features dominate.** GBDT (LightGBM) on lag features wins.
- **Latency-sensitive serving.** A linear model is 1000× faster.

### About the original linked package

Allen Chiang's `Time-Series-Transform` (`pip install time-series-transform`) is a **data preparation library**, not a model. It provides:

- `Time_Series_Transformer` — core class for arbitrary multi-dimensional TS data (slicing, lag/lead windowing, multi-category aggregation).
- `Stock_Transformer` — sub-class with built-in extraction from Yahoo / IEX-style sources and `pandas-ta` technical-indicator wiring.
- `make_lag` / `make_lead` / `make_lag_sequence` / `make_lead_sequence` — the four shifting helpers; the `_sequence` variants emit windowed arrays suitable as DL inputs.

It hasn't seen recent activity. For modern preprocessing, prefer `darts.utils.timeseries_generation` or `nixtla/utilsforecast`.

## Similar / related topics

- [[forecasting]] — broader Python forecasting overview
- [[kan]] — KAN-based alternative to MLP backbones
- [[time_sieve]] — wavelet / information-bottleneck non-Transformer architecture
- [[ml/time_series/papers|papers]] — broader paper reading list
- [[../llm/README|ml/llm]] — the NLP Transformer story this borrows from

## Internal links

- [[forecasting]]
- [[kan]]
- [[time_sieve]]
- [[linear_regression]]
- [[ml/time_series/papers|papers]]
- [[ml/time_series/tutorials|tutorials]]
- [[README]]

## Keywords

`#transformer` `#time-series` `#informer` `#autoformer` `#fedformer` `#patchtst` `#itransformer` `#dlinear` `#timesfm` `#chronos` `#lag-llama` `#moirai` `#foundation-models`

## References / raw notes

### Architecture papers (lineage)

- Vaswani et al., *Attention Is All You Need* (NeurIPS 2017) — <https://arxiv.org/abs/1706.03762>
- Zhou et al., *Informer* (AAAI 2021) — <https://arxiv.org/abs/2012.07436>
- Wu et al., *Autoformer* (NeurIPS 2021) — <https://arxiv.org/abs/2106.13008>
- Zhou et al., *FEDformer* (ICML 2022) — <https://arxiv.org/abs/2201.12740>
- Zeng et al., *Are Transformers Effective for Time Series Forecasting?* (AAAI 2023, the **DLinear** paper) — <https://arxiv.org/abs/2205.13504>
- Nie et al., *PatchTST* (ICLR 2023) — <https://arxiv.org/abs/2211.14730>
- Liu et al., *iTransformer* (ICLR 2024) — <https://arxiv.org/abs/2310.06625>

### Foundation models for TS (2024 wave)

- Das et al., *TimesFM* (Google) — <https://arxiv.org/abs/2310.10688>
- Ansari et al., *Chronos* (Amazon) — <https://arxiv.org/abs/2403.07815>
- Rasul et al., *Lag-Llama* — <https://arxiv.org/abs/2310.08278>
- Woo et al., *Moirai* (Salesforce) — <https://arxiv.org/abs/2402.02592>

### Implementation / library pointers

- `darts` — <https://unit8co.github.io/darts/> — Informer, Autoformer, NHiTS, TFT all in one API.
- `neuralforecast` (Nixtla) — <https://github.com/Nixtla/neuralforecast> — N-BEATS, NHiTS, PatchTST, iTransformer, TFT, Informer, Autoformer.
- HuggingFace `transformers` ships pretrained TimesFM, Chronos, Moirai for zero-shot use.

### About `Time-Series-Transform` (the linked package)

Code: <https://github.com/allen-chiang/Time-Series-Transformer>
Docs: <https://allen-chiang.github.io/Time-Series-Transformer/introduction.html>
Blog post: <https://jchiang1225.medium.com/a-new-data-approach-in-time-series-analysis-2d6c97f209cd>

Install:

```sh
pip install time-series-transform
# requires tensorflow and plotly
```

Two main modules:

- `Time_Series_Transformer` — core, user-defined TS data
- `Stock_Transformer` — stock extraction + technical indicators via `pandas-ta`

Quick taste — load a multi-feature series (Delhi climate dataset on Kaggle) and inspect it:

```python
from time_series_transform.transform_core_api import Time_Series_Transformer
import pandas as pd

df = pd.read_csv('DailyDelhiClimateTrain.csv')
tst = Time_Series_Transformer(df, timeSeriesCol='date')

# Slice the first three days
print(tst.time_series_data[:3])
# {'date': array([...], dtype=object),
#  'meantemp': array([10.0, 7.4, 7.17]),
#  'humidity': array([84.5, 92.0, 87.0]),
#  'wind_speed': array([0.0, 2.98, 4.63]),
#  'meanpressure': array([1015.67, 1017.8, 1018.67])}
```

Lag/lead helpers — `make_lead`, `make_lag` (point shifts) and `make_lead_sequence`, `make_lag_sequence` (windowed arrays for DL):

```python
tst.make_lead(['meantemp', 'wind_speed'], leadNum=3)
tst.make_lead_sequence(['meantemp', 'meanpressure'], windowSize=3)
```

For multi-dimensional data (one series per category) set `mainCategoryCol` so all transforms run per-group rather than across the boundary.

For the stock variant:

```python
from time_series_transform.stock_transform import Stock_Transformer

stock = Stock_Transformer.from_stock_engine_date(
    symbol='AAPL', start_date='2019-01-01', end_date='2020-01-01',
    engine='yahoo')
stock.get_technial_indicator(['MACD', 'EMA_10', 'BBANDS'])
```

Reference texts:

- Hyndman & Athanasopoulos, *Forecasting: Principles and Practice* — <https://otexts.com/fpp3/>
- Lim & Zohren, *Time Series Forecasting With Deep Learning: A Survey* (2021) — <https://arxiv.org/abs/2004.13408>
