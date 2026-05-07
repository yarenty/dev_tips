---
title: loggers — picker guide for the Rust `log` family
main_link: https://docs.rs/log/
keywords: [loggers, rust, log, env_logger, simplelog, log4rs, fern, tracing-subscriber]
status: reviewed
---

# loggers — picker guide for the Rust `log` family

**Main link:** <https://docs.rs/log/>

## Summary

Rust's logging story has two layers: the **`log` facade crate** (defines the `info!` / `error!` / `Log` trait) and a **chosen backend** that actually writes the lines somewhere. This article is the picker guide across the common backends. For *structured* logging with spans, fields, and async-aware context, see [[tracing]] — for a new application that's the better default.

## Insight

**Pick the backend that matches your shape:**

| Backend | When to pick |
|---|---|
| **[`env_logger`](https://crates.io/crates/env_logger)** | A CLI or small daemon. `RUST_LOG=info,my_crate=debug` filtering is the only knob you want. Most ubiquitous. |
| **[`simplelog`](https://github.com/Drakulix/simplelog.rs)** | You want both terminal and file output with a small zero-config API; no JSON, no rotation. |
| **[`log4rs`](https://docs.rs/log4rs/)** | You want log4j-shaped declarative YAML/TOML config: rolling files, multiple appenders, filters. Verbose but powerful. |
| **[`fern`](https://crates.io/crates/fern)** | Builder API; flexible formatting; popular middle-ground between env_logger and log4rs. |
| **[`pretty_env_logger`](https://crates.io/crates/pretty_env_logger)** | Coloured `env_logger` output for dev. |
| **[`flexi_logger`](https://crates.io/crates/flexi_logger)** | Rolling files, async writes, "kitchen-sink" alternative to log4rs. |
| **[`tracing-subscriber`](https://crates.io/crates/tracing-subscriber)** | You're using [[tracing]] (which is what most new code does). It includes a `tracing_log::LogTracer` shim that converts `log`-crate calls into tracing events — so you get the ecosystem of `log`-using libraries pulled into your tracing pipeline. |

**Rule of thumb (2024+)**: for a new binary, start with `tracing` + `tracing-subscriber` and only fall back to `env_logger` for tiny CLIs. For a library crate, depend on the `log` *facade* (not a backend) so the application chooses.

```rust
// log + simplelog example (preserved from raw notes)
use log::{info, warn, debug, trace};
#[macro_use] extern crate log;
use simplelog::*;

#[tokio::main]
async fn main() -> Result<()> {
    // terminal only
    TermLogger::init(LevelFilter::Debug, Config::default(),
                     TerminalMode::Mixed, ColorChoice::Auto).unwrap();

    // both terminal and log file
    CombinedLogger::init(vec![
        TermLogger::new(LevelFilter::Warn, Config::default(),
                        TerminalMode::Mixed, ColorChoice::Auto),
        WriteLogger::new(LevelFilter::Info, Config::default(),
                         File::create("my_rust_binary.log").unwrap()),
    ]).unwrap();
}
```

```rust
// env_logger minimal (preserved)
#[macro_use] extern crate log;

fn main() {
    env_logger::init();           // honours RUST_LOG=info,my_app=debug
    info!("starting up");
}
```

**Gotchas**: only one logger may be initialised per process — calling `init` twice panics. The `log` crate caches the `max_level` at init; runtime level changes need a backend that supports it (env_logger and tracing-subscriber both do via `EnvFilter::try_new`). Library code should *never* call `init()` — that's the application's job.

## Similar / related topics

- [[tracing]] — modern structured-logging primitive; what most new apps should use.
- [`slog`](https://github.com/slog-rs/slog) — the older structured logger; tracing has replaced it.
- [[rust_prometheus]] — for *metrics*, not logs.
- [[loki]] — Grafana Loki, the log-aggregation backend you'd ship logs *to*.
- [[open_telemetry]] — emit logs via OTel for OTel-shaped pipelines.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[tracing]] — the structured-logging successor; cross-reference.
- [[loki]] — log-aggregation backend.
- [[open_telemetry]] — OTel for log/metric/trace export.
- [[rust_prometheus]] — metrics analogue.

## Keywords

`#loggers` `#log` `#rust` `#env_logger` `#simplelog` `#log4rs` `#fern` `#tracing-subscriber`

## References / raw notes

- `log` facade: <https://docs.rs/log/>
- `simplelog`: <https://github.com/Drakulix/simplelog.rs>
- `log4rs`: <https://docs.rs/log4rs/latest/log4rs/>
- `env_logger`: <https://crates.io/crates/env_logger>
- `tracing` (structured-logging successor): <https://github.com/tokio-rs/tracing>
- `fern`: <https://crates.io/crates/fern>
- `flexi_logger`: <https://crates.io/crates/flexi_logger>
