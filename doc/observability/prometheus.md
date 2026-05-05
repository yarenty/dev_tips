---
title: prometheus
main_link: 
keywords: [prometheus, observability, brew, usr]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# prometheus

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[observability/grafana|grafana]] — grafana _(score 31.1)_
- [[node_exporter]] — node_exporter _(score 31.1)_
- [[openobserve]] — OpenObserve _(score 22.9)_
- [[visualization/grafana|grafana]] — Grafana _(score 19.8)_
- [[programming/rust/web/observability_on_wasm|observability_on_wasm]] — obsetrvability on wasm _(score 18.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#prometheus` `#observability` `#brew` `#services` `#usr` `#local`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# prometheus

When run from `brew services`, `prometheus` is run from
`prometheus_brew_services` and uses the flags in:
/usr/local/etc/prometheus.args

To start prometheus now and restart at login:
brew services start prometheus
Or, if you don't want/need a background service you can just run:
/usr/local/opt/prometheus/bin/prometheus_brew_services
