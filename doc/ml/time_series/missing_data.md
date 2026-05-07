---
title: Handling missing data in time series
main_link: https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed
keywords: [missing-data, time-series, imputation, forward-fill, interpolation, kalman, mice]
status: reviewed
review_date: 2026/05/03
---

# Handling missing data in time series

**Main link:** <https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed>

## Summary

Missing values are universal in real time-series data — sensor outages, holiday gaps, late-arriving records, joins across calendars with different frequencies. Most ML and forecasting models can't ingest NaNs, so something has to fill them. The right strategy depends on **why** values are missing (MCAR / MAR / MNAR), the gap length, the seasonality structure, and whether you're working with one series or a panel of many.

## Insight

The canonical strategies, roughly in order from cheapest-and-roughest to most-careful:

1. **`df.dropna()`** — the lazy default. Throws away whole rows. Fine if missingness is rare and random; a disaster if missingness is correlated with the target (sensor fails when it's hot → you systematically lose hot-day rows).
2. **Forward-fill / backward-fill** (`.ffill()`, `.bfill()`) — propagate the last/next valid value. Reasonable for slowly-changing variables (temperature, exchange rate) over short gaps; wrong for anything cyclic. **Forward-fill leaks the future when used carelessly in cross-validation** — make sure the fill happens before the split or per-fold.
3. **Linear / time / spline interpolation** (`.interpolate(method=...)`) — connect the dots. Pandas supports `linear`, `time` (calendar-aware), `quadratic`, `cubic`, `pchip`, `akima`, and `spline`. `time` is what you want when timestamps are irregular.
4. **Seasonal decomposition + interpolate residual** — STL-decompose, fill the residual (which is approximately stationary), recompose. Much better than naive interpolation for strongly seasonal series.
5. **Kalman smoothing** (state-space models, `statsmodels.tsa.statespace`) — principled imputation under a fitted state-space model; gives uncertainty bands too. The right answer for sensor data with known noise structure.
6. **MICE** (`sklearn.experimental.IterativeImputer`, R `mice` package) — multiple imputation by chained equations. Predict each column from the others iteratively. Best when you have many cross-sectional features and missingness in several of them.
7. **Model-aware** — XGBoost/LightGBM handle NaN natively (the split learns a default direction); Prophet handles missing observations directly; some neural-forecasting libraries (e.g. `darts`) have masking layers. Sometimes the best "imputation" is to not impute.

Panel/multi-series gotchas:

- **Don't impute across series boundaries.** `groupby(series_id).ffill()`, not bare `ffill()`. Forward-filling the last value of series A into the first row of series B is a classic bug.
- **Different frequencies, different padding.** Daily-vs-hourly joins create artificial NaNs that aren't really missing — use `resample(...).asfreq()` deliberately.
- **Document and mark.** Add an `is_imputed` indicator column; downstream models can learn that imputed rows are different.
- **Audit.** Always plot a missingness heatmap (`missingno.matrix(df)`) before you decide how to fill — a "random" gap pattern often turns out to be a stuck sensor at certain hours.

## Similar / related topics

- [[forecasting]] — forecasting models that need clean inputs
- [[linear_regression]] — lag features amplify NaN holes
- `missingno` — visualisation library for missingness patterns
- `statsmodels.tsa.statespace` — Kalman smoothing for principled imputation
- *Statistical Analysis with Missing Data* (Little & Rubin) — the canonical text on MCAR/MAR/MNAR

## Internal links

- [[forecasting]]
- [[linear_regression]]
- [[time_series_transformer]]
- [[ml/time_series/tutorials|tutorials]]
- [[README]]

## Keywords

`#missing-data` `#time-series` `#imputation` `#forward-fill` `#interpolation` `#kalman` `#mice` `#statsmodels` `#pandas`

## References / raw notes

- *How to Handle Missing Data for Time Series* — <https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed>
- pandas docs on missing data — <https://pandas.pydata.org/docs/user_guide/missing_data.html>
- `missingno` — <https://github.com/ResidentMario/missingno>
- `sklearn.experimental.IterativeImputer` — <https://scikit-learn.org/stable/modules/impute.html>
- Little & Rubin, *Statistical Analysis with Missing Data* (3rd ed., 2019)

The simplest possible drop-everything recipe (use sparingly):

```python
# Drop any and all nulls across all columns
df.dropna(inplace=True)
```

By default `pandas.dropna` searches across all columns and drops any row with a null anywhere. Parameters worth knowing:

- `subset=['col1', 'col2']` — only consider these columns.
- `thresh=N` — keep rows that have at least N non-null values.
- `how='all'` — drop only rows that are *entirely* null.

Per-series fill on a panel:

```python
df = df.sort_values(['series_id', 'ts'])
df['y'] = df.groupby('series_id')['y'].ffill().bfill()
```

Time-aware interpolation (irregular index):

```python
df = df.set_index('ts').sort_index()
df['y'] = df['y'].interpolate(method='time', limit=6, limit_direction='both')
```
