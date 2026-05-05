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

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rust_prometheus]] — rust-prometheus form tikv _(score 48.5)_
- [[loggers]] — Loggers _(score 31.9)_
- [[open_telemtry]] — open telemtry _(score 23.0)_
- [[programming/rust/tooling/observability_on_wasm|observability_on_wasm]] — obsetrvability on wasm _(score 23.0)_
- [[loki]] — Loki _(score 23.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#tracing` `#tooling` `#rust` `#programming` `#enable` `#metric` `#tokio` `#prometheus`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 4 additional top-level heading(s) extracted into sibling files:
> - [rust-prometheus form tikv](rust_prometheus.md)
> - [Loki](loki.md)
> - [open telemtry](open_telemtry.md)
> - [obsetrvability on wasm](observability_on_wasm.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Tracing

https://github.com/tokio-rs/tracing

tracing is a framework for instrumenting Rust programs to collect structured, event-based diagnostic information. tracing is maintained by the Tokio project, but does not require the tokio runtime to be used.
