---
title: rust_prometheus — TiKV's Prometheus client crate
main_link: https://github.com/tikv/rust-prometheus
keywords: [rust-prometheus, tikv, prometheus, metrics, observability, exporter, rust]
status: reviewed
---

# rust_prometheus — TiKV's Prometheus client crate

**Main link:** <https://github.com/tikv/rust-prometheus>

## Summary

[`prometheus`](https://crates.io/crates/prometheus) is the canonical Rust Prometheus client library, originally extracted from [TiKV](https://github.com/tikv/tikv) and now maintained at <https://github.com/tikv/rust-prometheus>. It provides counters, gauges, histograms, summaries, and a `TextEncoder` that produces the standard `/metrics` Prometheus exposition format. The companion crate [`prometheus-static-metric`](https://crates.io/crates/prometheus-static-metric) reduces the per-call overhead of `MetricVec` lookups when the label values are known at compile time.

## Insight

Reach for the `prometheus` crate when you want your Rust service to expose a `/metrics` endpoint that a Prometheus server (or [[../../../observability/openobserve|OpenObserve]] / Grafana Mimir / VictoriaMetrics / Datadog Agent) scrapes. The standard pattern:

```rust
use prometheus::{IntCounter, register_int_counter, TextEncoder, Encoder};

lazy_static::lazy_static! {
    static ref REQUESTS: IntCounter =
        register_int_counter!("http_requests_total", "Total HTTP requests").unwrap();
}

async fn handle_metrics() -> impl axum::response::IntoResponse {
    let mut buf = Vec::new();
    TextEncoder::new()
        .encode(&prometheus::gather(), &mut buf)
        .unwrap();
    (
        [(axum::http::header::CONTENT_TYPE, "text/plain; version=0.0.4")],
        buf,
    )
}

// In your handler:
REQUESTS.inc();
```

**Crate features**:

- `gen` — regenerate protobuf bindings from a fresh protoc.
- `nightly` — nightly-only optimisations (specialisation).
- `process` — adds `process_*` metrics (CPU, RSS, fds) automatically.
- `push` — Pushgateway client (only useful for short-lived batch jobs).

**Compared to siblings**:

- **[`metrics`](https://crates.io/crates/metrics) + `metrics-exporter-prometheus`** — the more idiomatic *facade* pattern (akin to `log` ↔ `env_logger`). Library code calls `metrics::counter!(...)`; the application chooses the exporter. **For a new project starting today this is usually the better choice** — it lets you switch backends without re-instrumenting.
- **[`opentelemetry-prometheus`](https://crates.io/crates/opentelemetry-prometheus)** — bridge from the OTel Meter API to a Prometheus scrape endpoint. Use when you want OTel for traces and Prometheus for metrics in the same pipeline.
- **`prometheus-client`** — the official Prometheus-Rust client (separate from this TiKV-derived one); aimed at OpenMetrics; smaller, cleaner API but less battle-tested.

**When to pick which**: TiKV's `prometheus` is the most-deployed and the safest pick if you want maximum ecosystem compatibility (many crates' `prometheus` features integrate with it directly). `metrics` + exporter is the more flexible *facade* pick. `prometheus-client` is the standards-aligned newcomer.

For the Prometheus *server* (PromQL, retention, alertmanager, federation), see [[../../../observability/prometheus|observability/prometheus]].

## Similar / related topics

- [`metrics`](https://crates.io/crates/metrics) + [`metrics-exporter-prometheus`](https://crates.io/crates/metrics-exporter-prometheus) — facade pattern; often the better starting point.
- [`prometheus-client`](https://crates.io/crates/prometheus-client) — official client; OpenMetrics-aligned.
- [`opentelemetry-prometheus`](https://crates.io/crates/opentelemetry-prometheus) — OTel-Meter bridge.
- [[../../../observability/prometheus|prometheus]] — the server-side article.
- [[tracing]] / [[open_telemetry]] — for spans/traces; pair with metrics.

## Internal links

- [[README]] — tooling section landing.
- [[../../../observability/prometheus|observability/prometheus]] — Prometheus server / PromQL.
- [[tracing]] / [[open_telemetry]] — sibling observability primitives.
- [[loki]] — log-side analogue.

## Keywords

`#rust-prometheus` `#tikv` `#prometheus` `#metrics` `#observability` `#exporter` `#rust`

## References / raw notes

- Repo: <https://github.com/tikv/rust-prometheus>
- Crate: <https://crates.io/crates/prometheus>
- Docs: <https://docs.rs/prometheus>
- Static-metric companion: <https://crates.io/crates/prometheus-static-metric>
- Crate features:
  - `gen` — regenerate protobuf client with the latest protoc.
  - `nightly` — nightly-only optimisations.
  - `process` — process metrics (CPU/RSS/fds).
  - `push` — Pushgateway support.
- Static metric: when label values are known at compile time, `prometheus-static-metric` reduces overhead of looking up the concrete `Metric` from a `MetricVec` (see `static-metric/` in the repo).
