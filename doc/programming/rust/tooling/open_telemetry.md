---
title: open_telemetry — OpenTelemetry SDK for Rust
main_link: https://github.com/open-telemetry/opentelemetry-rust
keywords: [open-telemetry, otel, rust, opentelemetry, tracing, opentelemetry-otlp, observability]
status: reviewed
review_date: 2026/05/03
---

# open_telemetry — OpenTelemetry SDK for Rust

**Main link:** <https://github.com/open-telemetry/opentelemetry-rust>

## Summary

[OpenTelemetry](https://opentelemetry.io/) (OTel) is the CNCF-graduated **vendor-neutral observability standard** — one set of APIs and protocols for traces, metrics, and logs that exports to any compliant backend (Jaeger, Tempo, Datadog, Honeycomb, NewRelic, AWS X-Ray, Azure Monitor, GCP Cloud Trace, etc.). The Rust implementation is split across:

- **[`opentelemetry`](https://crates.io/crates/opentelemetry)** — the core API + SDK (Tracer, Meter, Logger).
- **[`opentelemetry-otlp`](https://crates.io/crates/opentelemetry-otlp)** — the OTLP exporter (gRPC or HTTP) — what almost everyone uses to talk to a collector or a backend that natively speaks OTLP.
- **[`opentelemetry-stdout`](https://crates.io/crates/opentelemetry-stdout)** — print-to-stdout exporter for dev.
- **[`tracing-opentelemetry`](https://crates.io/crates/tracing-opentelemetry)** — bridge from [[tracing]] spans → OTel spans. The most-used integration.
- **Backend-specific crates** — `opentelemetry-jaeger`, `opentelemetry-zipkin`, `opentelemetry-prometheus`, etc. — direct exporters for individual backends.

## Insight

The clearest mental model: **`tracing` emits, OTel exports**.

- `tracing` is the *instrumentation API* you sprinkle through your Rust code (`#[instrument]`, `info!`, span guards). It is opinionated and lovely to use day-to-day.
- OpenTelemetry is the *transport and protocol* — boring SDK code that batches spans and posts them to an OTLP endpoint.

You bridge them with `tracing-opentelemetry` and end up with one Rust API (`tracing`) that drops into any OTel-compatible backend.

Standard wiring:

```rust
use opentelemetry_otlp::WithExportConfig;
use tracing_subscriber::{layer::SubscriberExt, util::SubscriberInitExt};

let tracer = opentelemetry_otlp::new_pipeline()
    .tracing()
    .with_exporter(
        opentelemetry_otlp::new_exporter()
            .tonic()
            .with_endpoint("http://localhost:4317"),     // OTLP/gRPC
    )
    .install_batch(opentelemetry_sdk::runtime::Tokio)?;

let otel_layer = tracing_opentelemetry::layer().with_tracer(tracer);

tracing_subscriber::registry()
    .with(tracing_subscriber::EnvFilter::from_default_env())
    .with(tracing_subscriber::fmt::layer())              // stdout for humans
    .with(otel_layer)                                    // OTLP for backends
    .init();
```

**Gotchas**:

- The Rust SDK is **API-stable but the SDK crate version churns**; always pin minor versions and the `opentelemetry-otlp` major must match the `opentelemetry` major.
- Default export is async-batched — if your process exits without `opentelemetry::global::shutdown_tracer_provider()`, you lose unflushed spans.
- **Logs** in OTel-Rust are still less mature than traces and metrics; for log-shaped data, ship via [[loki]]/Vector or via `tracing-bunyan-formatter` to a JSON-log collector for now.
- Sampling configuration matters — `Sampler::AlwaysOn` in production will overwhelm any backend; pick `ParentBased(TraceIdRatioBased)` and tune the ratio.

For *metrics* specifically, the Rust OTel-metrics API is workable but not yet as ergonomic as [[rust_prometheus|the prometheus crate]]; many Rust services still expose Prometheus directly and use OTel only for traces. The OpenTelemetry Collector (run as a sidecar) can ingest Prometheus-format metrics and re-emit them as OTLP — this is a common bridging pattern.

## Similar / related topics

- [[tracing]] — the instrumentation API; pair via `tracing-opentelemetry`.
- [[rust_prometheus]] — Prometheus client; the alternative for metrics.
- [Jaeger](https://www.jaegertracing.io/) / [Tempo](https://grafana.com/oss/tempo/) / Datadog / Honeycomb / NewRelic — backends OTel can export to.
- [OpenTelemetry Collector](https://opentelemetry.io/docs/collector/) — universal sidecar that translates OTLP ↔ everything.
- [[../web/observability_on_wasm|observability_on_wasm]] — Wasm-guest tracing (Dylibso observe-sdk).

## Internal links

- [[README]] — tooling section landing.
- [[tracing]] — instrumentation API.
- [[rust_prometheus]] — metrics alternative / complementary.
- [[loki]] — Loki for logs.
- [[../../../observability/README|observability/]] — broader observability stack.

## Keywords

`#open-telemetry` `#otel` `#rust` `#opentelemetry` `#tracing` `#opentelemetry-otlp` `#observability`

## References / raw notes

- Repo: <https://github.com/open-telemetry/opentelemetry-rust>
- Ecosystem index: <https://github.com/open-telemetry/opentelemetry-rust#ecosystem>
- Rust language docs: <https://opentelemetry.io/docs/instrumentation/rust/>
- OTLP exporter: <https://crates.io/crates/opentelemetry-otlp>
- tracing bridge: <https://crates.io/crates/tracing-opentelemetry>
