---
title: Observability
keywords: [observability, prometheus, grafana, node-exporter, openobserve, metrics, logs, traces]
status: reviewed
review_date: 2026/05/03
---

# Observability

Articles on the metrics + logs + traces stack. The classic shape is **Prometheus + Grafana + node_exporter** — small, opinionated, infinitely extended via the [Prometheus exporter zoo](https://prometheus.io/docs/instrumenting/exporters/). [[openobserve]] is the "single-Rust-binary alternative to all of it" if your team doesn't want to operate the whole stack.

## Decision tree

| What you need | Reach for |
|---------------|-----------|
| Metrics scraping + alerting | [[prometheus]] |
| Dashboards on top | [[observability/grafana|grafana]] (operator's view) or [[visualization/grafana|grafana]] (dashboard-design view) |
| Host-level metrics on every Linux box | [[node_exporter]] |
| All of logs + metrics + traces in one binary, willing to leave the standard stack | [[openobserve]] |
| Long-term metrics storage past Prometheus's ~15-day default | [Mimir](https://grafana.com/oss/mimir/), [Thanos](https://thanos.io/), or [VictoriaMetrics](https://victoriametrics.com/) — see [[visualization/grafana|grafana]] for context |
| Logs aggregation in the Grafana stack | [Loki](https://grafana.com/oss/loki/) |
| Distributed tracing in the Grafana stack | [Tempo](https://grafana.com/oss/tempo/) |

## Articles

- [[prometheus]] — pull-based metrics + PromQL. Pull semantics, ~15-day local retention, exporter zoo, recording rules, cardinality footguns.
- [[observability/grafana|grafana]] — operator's view: install (brew + Docker), provisioning datasources / dashboards as YAML, external-DB HA, foot-guns. (For the dashboard-design + Mimir framing of the same product, see [[visualization/grafana|grafana]].)
- [[node_exporter]] — host-level Prometheus exporter. Install recipes, collector tuning, `textfile` collector for cron-job metrics, Grafana dashboard 1860.
- [[openobserve]] — Rust single-binary all-in-one (logs + metrics + traces + RUM). Parquet-on-S3 storage; ELK ingest compatibility; OTLP-native.

## Cross-section see-also

- [[visualization/grafana|grafana]] — same Grafana product framed as a visualisation tool + Mimir long-term storage.
- [[k3s]] — natural Kubernetes substrate to run any of these on (especially via the [`kube-prometheus-stack`](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) Helm chart).
- [[oracle_free_tier]] — natural target to host an entire Prometheus + Grafana + Loki + Tempo stack for free.
- [[messaging/mqtt|mqtt]] / [[kafka]] / [[redpanda]] — the kinds of services you'll be monitoring with this stack.
- [[shodan]] — flips the question: what does *your* infrastructure look like from the outside?

## Keywords

`#observability` `#prometheus` `#grafana` `#node-exporter` `#openobserve` `#metrics` `#logs` `#traces` `#opentelemetry`
