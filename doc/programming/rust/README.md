---
title: Rust
keywords: [rust, cargo, ecosystem, async, embedded, web, ml, data]
status: reviewed
---

# Rust

By far the largest section of this vault — roughly a 50:1 ratio of articles vs any other language section under `programming/`. That reflects what I actually reach for day-to-day, not a verdict on Rust being uniquely worth writing about. Treat this as a curated, opinionated index over the parts of the Rust ecosystem I've spent real time in: language fundamentals, async/concurrency, data, web, GUI, TUI, CLI, embedded, ML bindings, SQL/database internals, and the wider tooling ecosystem.

## Where to start

| You want to… | Go to |
|--------------|-------|
| Learn the language / find books / cheat sheets | [[learning/README|learning]] |
| Understand error handling, generational indices, pinning, reflection | [[core/README|core]] |
| Pick between threads / async / channels / actors / dataflow | [[concurrency/README|concurrency]] |
| Build a CLI tool (clap, prompts, progress bars) | [[cli/README|cli]] |
| Build a full-screen terminal app (Ratatui, Cursive) | [[tui/README|tui]] |
| Build a desktop or mobile GUI (Tauri, egui, Slint, Iced, Dioxus) | [[gui/README|gui]] |
| Write an HTTP server / WASM / web client (axum, hyper, reqwest, wasmtime) | [[web/README|web]] |
| Wire up gRPC / MQTT / NATS / TCP tunnels | [[net/README|net]] |
| Read/write JSON, Excel, HDFS, Parquet, parsers | [[io/README|io]] |
| Talk to a database / build query engines (Diesel, DataFusion, sqlparser, …) | [[data/README|data]] |
| Build a SQL engine, UDF, or DB protocol | [[sql_engine/README|sql_engine]] |
| Call Python / Java / .NET from Rust (or vice-versa) | [[interop/README|interop]] |
| Train / run ML models (Linfa, CUDA, candle, burn — see also `ml/`) | [[ml/README|ml]] |
| Find a *built-in-Rust* CLI / dev tool to install (bottom, ripgrep, bat, exa, starship, …) | [[tooling/README|tooling]] |
| Embedded / no-std / RTIC / Embassy / Raspberry Pi | [[misc/README|misc]] (audio, embedded, Windows-RS) |
| Plugin systems for Rust hosts | [[plugins/README|plugins]] |

## Subsections

- [[learning/README|learning]] — books, tutorials, cheat sheets, project ideas.
- [[core/README|core]] — language-level patterns: error handling, hashbrown/SwissTable, pin-projection, reflection, generational indices, tinyvec.
- [[concurrency/README|concurrency]] — sync (crossbeam, rayon) vs async (tokio); actors, salsa, timely, par-stream.
- [[cli/README|cli]] — argument parsing (clap), prompts (dialoguer), progress (indicatif), colour, error pretty-printing.
- [[tui/README|tui]] — full-screen terminal apps: Ratatui (default), Cursive, termion, iocraft, television.
- [[gui/README|gui]] — desktop & mobile GUIs: Tauri, egui, Slint, Iced, Dioxus, Apple bindings.
- [[web/README|web]] — axum, hyper, reqwest, Rocket, wasmtime, WebAssembly, tungstenite, Yew.
- [[net/README|net]] — gRPC (tonic), MQTT (rumqtt, paho-mqtt), NATS, tRPC analogues, tunnelling.
- [[io/README|io]] — JSON, parsers, bytemuck, spreadsheets, HDFS, storage.
- [[data/README|data]] — database clients & data libraries: diesel, deadpool/r2d2/bb8/mobc, sqlparser, surrealDB, GreptimeDB, DataFusion ecosystem (blaze, delta, gluten, iceberg, vortex), Lance, Meilisearch.
- [[sql_engine/README|sql_engine]] — building SQL engines: parser, planner, UDFs (Excel/Hive/Spark), backends (CrateDB, Databend, MariaDB, MS Server, Snowflake, sqlite-loadable).
- [[interop/README|interop]] — bindings to Python (pyo3, maturin, PUFF, ballista-py), Java, .NET.
- [[ml/README|ml]] — Rust-side ML: CUDA, Linfa, the broader ML-in-Rust landscape.
- [[tooling/README|tooling]] — *end-user* CLIs/TUIs built in Rust: bottom, ripgrep, bat, exa, starship, zellij, mprocs, lychee, watchexec, joshuto, …
- [[misc/README|misc]] — embedded (Embassy, RTIC, raspberry_pi), audio (CoreAudio, Symphonia, Rodio, lewton), Windows-RS, charts, algorithms.
- [[plugins/README|plugins]] — plugin systems for Rust hosts.

## See also (cross-section)

- [[../README|programming]] — parent landing page.
- [[../../db/README|db]] — for database / query / format reference; many `data/` and `sql_engine/` articles cross-reference it.
- [[../../ml/README|ml]] — wider ML/LLM/agent material lives there; this section's `ml/` is just the Rust-bindings slice.
- [[../../tools/README|tools]] — generic dev tooling (editors, multiplexers, networking, security) that pairs well with Rust workflows.

## Keywords

`#rust` `#cargo` `#ecosystem` `#async` `#embedded` `#web` `#ml` `#data`
