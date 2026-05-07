---
title: tracing — structured spans + events for Rust
main_link: https://github.com/tokio-rs/tracing
keywords: [tracing, rust, tokio, observability, structured-logging, spans, opentelemetry]
status: reviewed
---

# tracing — structured spans + events for Rust

**Main link:** <https://github.com/tokio-rs/tracing>

## Summary

`tracing` is the canonical Rust observability primitive — a framework for instrumenting Rust programs to collect *structured*, event-based diagnostic information. It's maintained by the Tokio project but is runtime-agnostic (works in sync code, in `tokio`, `async-std`, `smol`). The library splits into:

- **`tracing`** — the `info!` / `error!` / `#[instrument]` macros and the *span* abstraction.
- **`tracing-subscriber`** — the consumer side: `fmt::Subscriber` for human-readable logs, `EnvFilter` for `RUST_LOG`-style filtering, JSON output, custom layers.
- **Layers** — composable pieces: `tracing-opentelemetry` (OTel export), `tracing-bunyan-formatter`, `tracing-loki`, `tracing-tree`, `tracing-flame`, `console-subscriber` (tokio-console), `tracing-appender` (rolling files).

## Insight

Reach for `tracing` for **any new Rust binary that does I/O** — it has effectively replaced `log` + `env_logger` as the default. The killer features over plain `log`:

1. **Structured fields**: `info!(user_id = 42, latency_ms = 13, "request done")` — fields are first-class, not part of the format string. JSON layers preserve types.
2. **Spans**: a request handler creates a span; everything inside (including async `.await`) is automatically annotated with the request's id/path/user/etc. Visible across thread / task hops.
3. **Async-aware**: spans correctly follow tasks across `await` boundaries (this is what `log` cannot do).
4. **Pluggable export**: same instrumentation, different consumers — pretty stdout in dev, JSON+OTel in prod.

Standard application setup:

```rust
use tracing_subscriber::{fmt, EnvFilter, prelude::*};

fn main() {
    tracing_subscriber::registry()
        .with(EnvFilter::from_default_env()        // RUST_LOG=info,my_app=debug
            .add_directive("hyper=warn".parse().unwrap()))
        .with(fmt::layer().with_target(false))
        .init();

    tracing::info!("starting up");
}

#[tracing::instrument(skip(db))]
async fn handle_request(db: &Database, user_id: u64) -> Result<User> {
    tracing::info!("looking up user");
    db.get_user(user_id).await
}
```

**Compared to siblings**:

- **[[loggers|`log` + `env_logger`]]** — the older, simpler primitive. Use only for tiny CLIs that don't need spans.
- **`slog`** — the structured-logging predecessor; tracing has more momentum.
- **[[open_telemetry|OpenTelemetry SDK directly]]** — the *exporter* side; pair with tracing via `tracing-opentelemetry`. The standard mental model: **tracing emits, OTel exports**.

**Gotchas**: `#[instrument]` adds a span per call — overhead is non-zero, don't put it on hot inner-loop functions; use `skip(self)` to keep large fields out of formatted output (and to avoid `Debug`-impl explosion); `tokio-console` requires the `console-subscriber` layer + `tokio_unstable` flag. For *log* output specifically (lines, not spans), `tracing` works — but if you want `RUST_LOG`-style filtering only and don't need spans, the lighter `[[loggers|env_logger]]` may be enough.

## Similar / related topics

- [[loggers]] — picker guide for the `log` family (env_logger, simplelog, log4rs).
- [[open_telemetry]] — the exporter side; pair with `tracing-opentelemetry`.
- [[rust_prometheus]] — metrics-side analogue (counters/gauges/histograms).
- `tokio-console` — runtime introspection for `tokio` tasks; uses tracing under the hood.
- `slog` — predecessor structured-logging crate.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[loggers]] — sibling article on the `log` family.
- [[open_telemetry]] — pair via `tracing-opentelemetry`.
- [[rust_prometheus]] — metrics analogue.
- [[loki]] — Grafana Loki notes (log-aggregation backend).
- [[../web/observability_on_wasm|observability_on_wasm]] — Wasm-guest tracing (canonical home in `web/`).

## Keywords

`#tracing` `#rust` `#tokio` `#observability` `#structured-logging` `#spans` `#opentelemetry`

## References / raw notes

- Repo: <https://github.com/tokio-rs/tracing>
- Crate: <https://crates.io/crates/tracing>
- Subscriber: <https://crates.io/crates/tracing-subscriber>
- Companion crates: `tracing-opentelemetry`, `tracing-bunyan-formatter`, `tracing-loki`, `tracing-tree`, `tracing-flame`, `console-subscriber`, `tracing-appender`.

Related Rust observability articles (cross-link map; previously a sibling-link block from the auto-split):

- [[rust_prometheus]] — TiKV's Prometheus client.
- [[loki]] — Grafana Loki.
- [[open_telemetry]] — OpenTelemetry SDK.
- [[../web/observability_on_wasm|observability_on_wasm]] — Wasm-guest observability.
