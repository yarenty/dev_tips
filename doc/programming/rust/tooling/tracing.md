---
title: Tracing
main_link: https://github.com/tokio-rs/tracing
keywords: [tracing, tooling, rust, programming, enable, metric, tokio, prometheus]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Tracing

**Main link:** <https://github.com/tokio-rs/tracing>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#tracing` `#tooling` `#rust` `#programming` `#enable` `#metric` `#tokio` `#prometheus`

## TODO

- This file contains **5 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Tracing

https://github.com/tokio-rs/tracing

tracing is a framework for instrumenting Rust programs to collect structured, event-based diagnostic information. tracing is maintained by the Tokio project, but does not require the tokio runtime to be used.




# rust-prometheus form tikv

https://github.com/tikv/rust-prometheus


Find the latest documentation at https://docs.rs/prometheus.

## Crate features
This crate provides several optional components which can be enabled via Cargo [features]:

- gen: To generate protobuf client with the latest protobuf version instead of using the pre-generated client.

- nightly: Enable nightly only features.

- process: Enable process metrics support.

- push: Enable push metrics support.

Static Metric
When using a MetricVec with label values known at compile time prometheus-static-metric reduces the overhead of retrieving the concrete Metric from a MetricVec.

See static-metric directory for details.


# Loki


inspired by prometeus

quite interesting




# open telemtry


https://opentelemetry.io/docs/instrumentation/rust/


https://github.com/open-telemetry/opentelemetry-rust#ecosystem





# obsetrvability on wasm

https://github.com/dylibso/observe-sdk

https://github.com/dylibso/observe-sdk/tree/main/rust
