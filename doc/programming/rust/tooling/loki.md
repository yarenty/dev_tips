---
title: loki — Grafana Loki (Rust integration notes)
main_link: https://grafana.com/oss/loki/
keywords: [loki, grafana, prometheus, logs, log-aggregation, rust, tracing-loki]
status: reviewed
review_date: 2026/05/03
---

# loki — Grafana Loki (Rust integration notes)

**Main link:** <https://grafana.com/oss/loki/>

## Summary

[Grafana Loki](https://grafana.com/oss/loki/) is a horizontally-scalable, multi-tenant **log aggregation system** by Grafana Labs, deliberately modelled on [Prometheus](https://prometheus.io/) — same labels, same architecture, same operator mental model — but for logs instead of metrics. Loki indexes only metadata (labels), not log content, which keeps storage cheap. This article focuses on the **Rust-side integration**: how to ship log lines from a Rust service into a Loki instance.

## Insight

The Rust integration story for Loki is small but useful:

- **[`tracing-loki`](https://crates.io/crates/tracing-loki)** — a [[tracing]] subscriber layer that pushes spans/events to Loki's HTTP API. The most idiomatic path if you've already standardised on tracing.
- **Promtail / Vector / Fluent Bit sidecar** — log to stdout/stderr or a file, let an agent ship it. This is the *recommended* shape for production: keep the application simple, let the log-pipeline tool handle batching, retries, and label enrichment. **Vector** in particular is itself a Rust project and pairs cleanly here.
- **Direct HTTP push** — Loki's [push API](https://grafana.com/docs/loki/latest/reference/loki-http-api/#ingest-logs) is a JSON POST to `/loki/api/v1/push`; trivial to call from `reqwest`. Use only for special cases (one-off scripts, CI uploaders).

Operator's view of Loki itself (single-binary mode, microservices mode, retention, S3 backend, the Mimir/Tempo/Loki "LGTM" stack) lives at the broader observability article — see [[../../../observability/README|observability/]] and pair with [[../../../observability/grafana|grafana]].

**Compared to siblings**:

- **ELK / Elasticsearch + Kibana** — the older incumbent; rich full-text search but expensive to operate.
- **OpenSearch** — Amazon's Elasticsearch fork; same shape, fewer license headaches.
- **[[../../../observability/openobserve|OpenObserve]]** — the modern Rust single-binary alternative; covers logs + metrics + traces with much less ops.
- **CloudWatch / Stackdriver / Azure Monitor** — managed cloud equivalents.

**When to pick Loki**: you already run Grafana for dashboards, your log volume is "moderate" (TB/day, not PB/day), and the *labels* are how you'd actually search (e.g. `{app="payments", env="prod", level="error"}`) rather than full-text content. **Skip Loki** if your search pattern is "find the line containing `userid=12345 amount=$54.32`" — Loki will work but slowly compared to ES or OpenObserve.

## Similar / related topics

- [[../../../observability/openobserve|openobserve]] — modern Rust single-binary alternative covering logs + metrics + traces.
- ELK / OpenSearch — the older full-text incumbents.
- [Vector](https://vector.dev/) — Rust observability data router; the recommended log-shipping sidecar.
- [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) — Loki's own log-shipping agent.

## Internal links

- [[README]] — tooling section landing.
- [[tracing]] — pair via `tracing-loki` subscriber.
- [[../../../observability/grafana|observability/grafana]] — the dashboards over the top.
- [[../../../observability/README|observability/]] — broader observability stack.
- [[../../../observability/openobserve|observability/openobserve]] — modern alternative covering all three pillars.

## Keywords

`#loki` `#grafana` `#prometheus` `#logs` `#log-aggregation` `#rust` `#tracing-loki` `#observability`

## References / raw notes

- Site: <https://grafana.com/oss/loki/>
- Repo: <https://github.com/grafana/loki>
- Rust client crate: `tracing-loki` (<https://crates.io/crates/tracing-loki>).
- Loki HTTP push API: <https://grafana.com/docs/loki/latest/reference/loki-http-api/>
- Inspired by Prometheus (same label model, same operator mental model).
