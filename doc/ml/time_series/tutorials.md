---
title: Time-series tutorials and learning resources
main_link: https://www.kaggle.com/learn/time-series
keywords: [tutorials, time-series, learning, kaggle, hyndman, fpp3, courses]
status: reviewed
review_date: 2026/05/03
---

# Time-series tutorials and learning resources

**Main link:** <https://www.kaggle.com/learn/time-series> (Ryan Holbrook, Kaggle Learn — *Time Series* mini-course)

## Summary

A small curated reading list for getting hands-on with time-series. The Kaggle Learn *Time Series* course (Ryan Holbrook, 5 lessons, free, ~5 hours total) is the headline practical pick — short, code-first, covers linear regression with TS features, trend, seasonality, cycles, and a hybrid baseline. For a serious foundation, Hyndman & Athanasopoulos's *Forecasting: Principles and Practice* (3rd edition, free online) is the field's de-facto textbook.

## Insight

Recommended progression for someone new to TS:

1. **Kaggle Learn — *Time Series* (Ryan Holbrook).** Five short notebooks. Builds intuition for trend, seasonality, lag features, and the hybrid models pattern. Good first ~5 hours.
2. **Hyndman & Athanasopoulos — *Forecasting: Principles and Practice* (3rd ed., free).** ETS, ARIMA, dynamic regression, hierarchical reconciliation, evaluation. R-flavoured (`fable` package) but the concepts transfer. <https://otexts.com/fpp3/>
3. **Nixtla tutorials.** `statsforecast`, `mlforecast`, `neuralforecast` — the most-active modern Python ecosystem. Their docs include cross-validation patterns, conformal-prediction intervals, hierarchical reconciliation, and zero-shot foundation-model use. <https://nixtla.github.io/>
4. **`darts` user guide.** Unified Python API across ~30 statistical, ML and DL models. Good for benchmarking. <https://unit8co.github.io/darts/>
5. **Marco Peixeiro's blog (Towards Data Science).** Practitioner-level walkthroughs of N-BEATS, NHiTS, TSMixer, TFT, KAN, foundation models. Code-heavy.
6. **Papers.** Once you've shipped one model, read the [[ml/time_series/papers|papers]] reading list (M-competitions, DLinear, the foundation-model wave).

Pitfalls to avoid in any tutorial:

- **Don't trust accuracy claims that use a single train/test split.** Walk-forward CV.
- **Be sceptical of stock-price tutorials.** They often look good because of trivial autocorrelation; they would not survive proper backtesting.
- **Compare against a naive baseline.** `y[t] = y[t-1]` (random walk) or `y[t] = y[t-7]` (weekly seasonal naive). If your fancy model doesn't beat both by a meaningful margin, the model isn't doing anything.

## Similar / related topics

- [[forecasting]] — applied forecasting overview
- [[linear_regression]] — the canonical first-tutorial topic
- [[ml/time_series/papers|papers]] — paper reading list
- [[time_series_transformer]] — Transformer-architecture deep-dive
- [[../fundamentals/courses|fundamentals/courses]] — broader ML learning resources

## Internal links

- [[forecasting]]
- [[linear_regression]]
- [[time_series_transformer]]
- [[kan]]
- [[time_sieve]]
- [[missing_data]]
- [[ml/time_series/papers|papers]]
- [[../fundamentals/courses|fundamentals/courses]]
- [[README]]

## Keywords

`#tutorials` `#time-series` `#learning` `#kaggle` `#hyndman` `#fpp3` `#courses` `#nixtla` `#darts`

## References / raw notes

### Free courses

- *Time Series* on Kaggle Learn (Ryan Holbrook, 5 lessons) — <https://www.kaggle.com/learn/time-series>
  - Lesson 1: *Linear Regression With Time Series* — <https://www.kaggle.com/code/ryanholbrook/linear-regression-with-time-series>
  - Lessons 2-5: trend, seasonality, hybrid models, time-series-as-features.

### Free books

- Hyndman & Athanasopoulos, *Forecasting: Principles and Practice* (3rd ed.) — <https://otexts.com/fpp3/>

### Library tutorials

- Nixtla docs (statsforecast, mlforecast, neuralforecast, hierarchicalforecast) — <https://nixtla.github.io/>
- darts user guide — <https://unit8co.github.io/darts/>
- sktime examples — <https://www.sktime.net/en/stable/examples.html>
- HuggingFace TS tutorials (TimesFM, Chronos, Moirai zero-shot) — <https://huggingface.co/docs/transformers/main/tasks/time_series_forecasting>

### Practitioner blogs

- Marco Peixeiro on Towards Data Science — N-BEATS, NHiTS, TSMixer, TFT, KAN walkthroughs.
- *Are Transformers Effective for Time Series Forecasting?* — required reading for context: <https://arxiv.org/abs/2205.13504>

(Merged from former `tutorials_2.md`, which was an empty auto-split fragment with no distinct content.)
