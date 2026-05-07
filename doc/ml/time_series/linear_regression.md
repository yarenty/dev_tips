---
title: Linear regression with time-series features
main_link: https://www.kaggle.com/code/ryanholbrook/linear-regression-with-time-series
keywords: [linear-regression, time-series, baseline, lag-features, rolling-features, kaggle]
status: reviewed
---

# Linear regression with time-series features

**Main link:** <https://www.kaggle.com/code/ryanholbrook/linear-regression-with-time-series> (Ryan Holbrook, *Linear Regression With Time Series* — Kaggle Learn)

## Summary

A classic time-series-on-rails recipe: build a feature matrix from the past — lag features (`y[t-1]`, `y[t-2]`, ...), rolling windows (mean, std, min, max), calendar features (hour-of-day, day-of-week, month, holidays), and a trend term — then fit ordinary least squares. Despite being unfashionable, this is the **boring baseline you should always run first**. It's interpretable, fits in milliseconds, has decades of statistical theory behind its confidence intervals, and tells you immediately whether your fancy DL model is actually doing anything.

## Insight

- **The boring baseline that wins more often than people admit.** Many "DL beats baseline" benchmarks compare against a *bad* baseline (no calendar features, no rolling stats). A linear regression on properly engineered features matches or beats DL on a depressing fraction of business-forecasting tasks.
- **Two flavours.** (1) Direct: predict `y[t+h]` from features at time `t`. (2) Recursive: predict `y[t+1]`, feed it back, predict `y[t+2]`. Direct is simpler and more honest; recursive gives smoother multi-step forecasts but accumulates error.
- **Feature pitfalls.**
  - **Leakage** — your rolling mean must use only past observations (`shift(1).rolling(7).mean()`, not `rolling(7).mean()`).
  - **Cold-start NaNs** — first `max(lag)` rows have no features. Drop them.
  - **Holiday calendars** — country-specific; `holidays` Python package is canonical.
- **Compare against:**
  - **ARIMA / SARIMA** — automatic lag selection; good when seasonality is regular.
  - **Prophet** — does the calendar/changepoint engineering for you, fits via Stan.
  - **LightGBM/XGBoost on the same features** — usually a bigger lift than going from linear to DL, given the same feature engineering.
  - **DL (LSTM/Transformer/N-BEATS)** — only justified if the linear-on-engineered-features baseline visibly fails.
- **Tooling.** `sklearn.linear_model.{LinearRegression, Ridge, Lasso}` for the model; `pandas` `shift`/`rolling`/`ewm` for features; `sktime.forecasting.compose.make_reduction` to wrap any sklearn regressor as a forecaster; or use `mlforecast` (Nixtla) which industrialises the lag-feature pipeline for many series at once.

## Similar / related topics

- [[forecasting]] — broader Python forecasting ecosystem
- [[time_series_transformer]] — the heavyweight DL alternative
- [[missing_data]] — must be handled before lag features (NaNs propagate)
- [[../fundamentals/no_deep_learning_needed|no_deep_learning_needed]] — same "GBDT-on-tabular-features beats DL" lesson
- Ryan Holbrook's *Time Series* Kaggle Learn course (5 lessons, free) — the canonical hands-on intro

## Internal links

- [[forecasting]]
- [[time_series_transformer]]
- [[missing_data]]
- [[ml/time_series/tutorials|tutorials]]
- [[../fundamentals/no_deep_learning_needed|fundamentals/no_deep_learning_needed]]
- [[README]]

## Keywords

`#linear-regression` `#time-series` `#baseline` `#lag-features` `#rolling-features` `#kaggle` `#sklearn` `#mlforecast`

## References / raw notes

- Ryan Holbrook, *Linear Regression With Time Series* — <https://www.kaggle.com/code/ryanholbrook/linear-regression-with-time-series>
- The full Kaggle Learn *Time Series* course (5 lessons): <https://www.kaggle.com/learn/time-series>
- `mlforecast` (Nixtla) — <https://github.com/Nixtla/mlforecast> — production-grade lag-feature pipelines.
- `sktime` reduction helpers — <https://www.sktime.net/>

Auto-split from `tutorials.md`.
