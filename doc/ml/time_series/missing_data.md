---
title: Drop any and all nulls across all columns
main_link: https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed
keywords: [missing-data, time-series, ml, data, handle, missing, drop]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Drop any and all nulls across all columns

**Main link:** <https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[articles]] — Articles _(score 20)_
- [[time_series_transformer]] — Time Series Transformer _(score 18)_
- [[time_serie_transformer]] — Transformers _(score 18)_
- [[forecasting]] — forecasting _(score 18)_
- [[ml/bigquery/links|links]] — Links _(score 15)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#missing-data` `#time-series` `#ml` `#data` `#handle` `#missing` `#drop`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->




## How to: Handle Missing Data for Time Series

https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed



There is no such thing as a perfect dataset. Every data scientist knows that feeling during data exploration when they call:

Most ML models cannot process NaN or null values, so it is important that if your features or target contain them, they are dealt with appropriately before attempting to fit a model to the data.

1. Drop nulls
   This is probably the simplest and most straightforward way to handle missing data: Just get rid of it.

# Drop any and all nulls across all columns
df.dropna(inplace=True)
By default, pandas’ dropna function searches for nulls across the board (in all columns) and drops any row where there is a null in any column. However, this can be modified using various parameters.
