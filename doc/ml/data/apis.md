---
title: API access to data
main_link: https://mixedanalytics.com/blog/list-actually-free-open-no-auth-needed-apis/
keywords: [apis, data, public, json, rest, no-auth, ml-data]
status: reviewed
---

# API access to data

**Main link:** <https://mixedanalytics.com/blog/list-actually-free-open-no-auth-needed-apis/>

## Summary

A curated grab-bag of public, no-auth (or trivial-auth) HTTP APIs that return useful or amusing JSON — anything from US census data, weather, public-holiday lookups, exchange rates and crypto tickers through to "random dog image". Useful as test fixtures, demo material for tutorials, dashboards, or as cheap real-world data sources for hobby ML projects when you don't want to register for a developer account just to make `curl` work.

## Insight

Most "free APIs" are actually freemium with sign-up walls; the value of a curated open-no-auth list is that you can `curl | jq` without an API key, which is exactly what you want for a 5-minute demo, a CI smoke test, or onboarding material. For real **ML datasets** at scale you want a proper hub — Hugging Face Datasets, Kaggle, OpenML, UCI ML Repository, data.gov, AWS Open Data — not these endpoints. For real **production data feeds** (weather, finance, geo) plan on signing up, paying, and respecting rate limits; the freebie endpoints in this list change, get rate-limited, or disappear without notice (e.g. CoinDesk's BPI was deprecated, the Bored API has gone offline before).

Gotcha pattern: many of these return CORS-restricted responses when called from a browser — they were built for server-side or `curl` use. Wrap behind your own backend if you need a JS frontend to consume them.

## Similar / related topics

- **Hugging Face Datasets** — the de-facto hub for ML training data, programmatic via `datasets` library.
- **Kaggle Datasets / Kaggle API** — competition-style + community datasets, CLI-driven download.
- **OpenML / UCI ML Repository** — classical tabular ML datasets with a REST API.
- **data.gov / data.gov.uk / EU Open Data Portal** — government open-data portals with mostly auth-free APIs.
- **public-apis/public-apis** — much larger but noisier GitHub list (~1500 entries, includes auth-required ones).

## Internal links

- [[public]] — broader curated list of ML/research-grade public datasets
- [[for_tests]] — tiny built-in datasets for unit tests / CI
- [[../../db/relational/postgresql|postgresql]] — when you've outgrown the API and want to ingest

## Keywords

`#apis` `#data` `#public` `#json` `#rest` `#no-auth` `#ml-data`

## References / raw notes

### Mixed Analytics — "Actually free, open, no-auth-needed APIs"

<https://mixedanalytics.com/blog/list-actually-free-open-no-auth-needed-apis/>

Selected entries (full list at the URL above; expect bit-rot — sample before relying on any single endpoint):

| Category | Example | Endpoint |
|----------|---------|----------|
| Weather | 7Timer! | `http://www.7timer.info/bin/api.pl?lon=113.17&lat=23.09&product=astro&output=json` |
| US public data | Data USA | `https://datausa.io/api/data?drilldowns=Nation&measures=Population` |
| Crypto tickers | Binance / Kraken / CoinGecko / Coinpaprika | `https://api.binance.com/api/v3/ticker/24hr` |
| FX rates | ExchangeRate-API | `https://open.er-api.com/v6/latest/USD` |
| Geo / IP | IPify, FreeGeoIP, IP2Country | `https://api.ipify.org?format=json` |
| Public holidays | Nager.Date | `https://date.nager.at/api/v2/publicholidays/2020/US` |
| Music | MusicBrainz | `http://musicbrainz.org/ws/2/artist/<mbid>?fmt=json` |
| Books | Open Library | `http://openlibrary.org/api/volumes/brief/isbn/9780525440987.json` |
| Food | Open Food Facts | `https://world.openfoodfacts.org/api/v0/product/<barcode>.json` |
| Wikipedia | Pageviews | `https://wikimedia.org/api/rest_v1/metrics/pageviews/per-article/...` |
| Whimsy | Numbers, Cocktails, Jokes, Dogs, xkcd | `http://numbersapi.com/random/math`, `https://dog.ceo/api/breeds/image/random` |
| Inspector | HTTPBin / JSONPlaceholder | `http://httpbin.org/get`, `https://jsonplaceholder.typicode.com/posts/1` |

### Bigger ML-data hubs (not in the no-auth list, but where you actually go for ML)

- Hugging Face Datasets — <https://huggingface.co/datasets>
- Kaggle Datasets — <https://www.kaggle.com/datasets>
- OpenML — <https://www.openml.org/>
- UCI ML Repository — <https://archive.ics.uci.edu/>
- AWS Open Data — <https://registry.opendata.aws/>
- data.gov — <https://www.data.gov/>
- Awesome Public Datasets — see [[public]]
