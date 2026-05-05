---
title: rust-prometheus form tikv
main_link: https://github.com/tikv/rust-prometheus
keywords: [rust-prometheus-form-tikv, rust, enable, metric, prometheus]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/tooling/tracing.md` by `article_split.py`. Heading: **rust-prometheus form tikv**.

# rust-prometheus form tikv

**Main link:** <https://github.com/tikv/rust-prometheus>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tracing]] — Tracing _(score 48.5)_
- [[starship]] — starship _(score 23.0)_
- [[programming/rust/tooling/tauri|tauri]] — TAURI _(score 23.0)_
- [[loki]] — Loki _(score 23.0)_
- [[prometheus]] — prometheus _(score 19.8)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#rust-prometheus-form-tikv` `#tooling` `#rust` `#programming` `#enable` `#metric` `#prometheus` `#static`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

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
