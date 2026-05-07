---
title: Rust developer tooling & built-with-Rust CLIs
keywords: [tooling, rust, cli, tui, observability, testing, replacements]
status: reviewed
review_date: 2026/05/03
---

# Rust developer tooling & built-with-Rust CLIs

Two overlapping populations of articles live here:

1. **CLIs/TUIs that happen to be written in Rust** — `bat`, `ripgrep`, `bottom`, `zoxide`, `starship`, `zellij`, etc. — many of which are now the de-facto modern replacements for classic Unix tools. This is the "Rust eats the GNU coreutils" trend.
2. **Rust-developer-specific tooling** — `tracing`, `loggers`, `cargo-tauri`, `evcxr` (REPL & Jupyter), `mockall`, `turmoil`, `app_builders` — things you reach for *because* you're writing Rust.

For broader cross-platform tool collections (not Rust-specific), see [[../../../tools/README|tools/]] (linux/, shell/, osx/ subsections). For deep-dives on the observability backends these tools talk to, see [[../../../observability/README|observability/]].

## Decision shortcuts

| Need | Reach for |
|---|---|
| Faster `grep` | `[[ripgrep]]` |
| Better `cat` | `[[bat]]` |
| Better `ls` | `[[exa]]` (unmaintained — use [`eza`](https://github.com/eza-community/eza) fork) |
| Better `du` | `[[dust]]` |
| Better `top`/`htop` | `[[bottom]]` (`btm` binary) |
| Bandwidth-by-process | `[[bandwhich]]` |
| Smarter `cd` | `[[zoxide]]` |
| Update everything at once | `[[topgrade]]` |
| Semantic / syntax-aware diff | `[[difftastic]]` |
| Markdown link checker | `[[lychee]]` |
| Cross-shell prompt | `[[starship]]` |
| Run a command on file change | `[[watchexec]]` |
| Terminal multiplexer (modern) | `[[zellij]]` |
| Multi-process TUI orchestrator | `[[mprocs]]` |
| Terminal file manager (Ranger clone) | `[[joshuto]]` |
| Rust REPL | `[[repl]]` (evcxr / iRust) |
| Rust in Jupyter | `[[rust_in_jupyter]]` |
| Structured logging in Rust | `[[tracing]]` |
| OpenTelemetry from Rust | `[[open_telemetry]]` |
| Prometheus metrics from Rust | `[[rust_prometheus]]` |
| Test-doubles / mocks | `[[tests\|mockall]]` |
| Deterministic distributed-system tests | `[[turmoil]]` |
| HTTP / temp-dir / binary test helpers | `[[tux]]` |
| Package a Tauri/desktop app | `[[app_builders]]` + `[[tauri]]` |

## Articles

### Replaces a classic Unix tool

- [[bat]] — `cat` clone with syntax highlighting and git-diff gutter.
- [[exa]] — `ls` replacement (note: **unmaintained since 2023**; the active fork is [`eza`](https://github.com/eza-community/eza)).
- [[dust]] — `du` replacement; tree-style disk usage.
- [[ripgrep]] — `grep`/`ag`/`ack` replacement; gitignore-aware by default.
- [[bottom]] — `top`/`htop` replacement (the `btm` binary).
- [[bandwhich]] — bandwidth-by-process top.
- [[joshuto]] — Ranger-style terminal file manager.
- [[zoxide]] — `cd` replacement (autojump-style).
- [[topgrade]] — orchestrates updates across all your package managers.
- [[difftastic]] — syntax-aware structural diff.
- [[lychee]] — async link checker for Markdown / HTML / source files.

### Multiplexers & process orchestration

- [[zellij]] — modern terminal multiplexer (tmux replacement). Cross-link: [[../../../tools/linux/tmux|tmux]].
- [[mprocs]] — TUI for running and monitoring multiple long-running processes (DBs, servers, log tails) side-by-side.

### Shell / dev productivity

- [[starship]] — cross-shell, language-aware prompt.
- [[watchexec]] — generic file-watch command runner (the engine behind `cargo-watch`).
- [[repl]] — `evcxr` / `iRust`: Rust REPL.
- [[clido]] — minimal CLI to-do list.

### Build / packaging

- [[app_builders]] — `awesome-app` template + the broader Rust desktop-app packaging story.

### Testing

- [[tests|mockall]] — mock-object generator for trait-based unit tests.
- [[tux]] — miscellaneous integration-test helpers (HTTP, temp dirs, binary execution).
- [[turmoil]] — Tokio team's deterministic distributed-system simulator.
- [[armada]] — high-performance TCP SYN scanner (filed here as a built-with-Rust CLI; security-tool angle in [[../../../tools/security/rustscan|rustscan]]).

### Observability / logging

- [[tracing]] — the canonical Rust observability primitive (structured spans + events).
- [[loggers]] — picker guide across `log` / `simplelog` / `log4rs` / `env_logger` / `tracing-subscriber`.
- [[open_telemetry]] — OpenTelemetry SDK for Rust; pairs with `tracing-opentelemetry`.
- [[rust_prometheus]] — TiKV's Prometheus client for exposing metrics.
- [[loki]] — Grafana Loki notes (log-aggregation backend; pairs with [[../../../observability/grafana|Grafana]]).
- [[debug]] — `Debug` derive helpers (`debug_stub_derive`, `derivative`).
- [[../web/observability_on_wasm|observability_on_wasm]] — Dylibso `observe-sdk` for tracing inside guest Wasm modules (canonical home is in `web/`).

### Specialised

- [[rust_in_jupyter]] — `evcxr_jupyter` kernel for Rust notebooks.
- [[uclicious]] — derive-driven config parser for the UCL (libucl) format.
- [[tauri]] — dev-tooling angle on Tauri (`cargo tauri` workflow); the framework overview lives at [[../gui/tauri|gui/tauri]].
- [[tools]] — historical "coreutils" parent file; mostly empty post-split.

## See also

- [[../../../tools/README|tools/]] — broader (non-Rust-specific) tool catalogue (linux/, shell/, osx/, security/, networking/, design/, editors/, misc/).
- [[../../../tools/linux/tmux|tools/linux/tmux]] — tmux deep-dive (the thing zellij replaces).
- [[../../../observability/README|observability/]] — Grafana, Prometheus, Node Exporter, OpenObserve.
- [[../gui/tauri|gui/tauri]] — canonical Tauri framework article.
- [[../web/webassembly|web/webassembly]] — Rust-Wasm landing page (cross-link target for `observability_on_wasm`).

## Keywords

`#tooling` `#rust` `#cli` `#tui` `#observability` `#testing` `#replacements`
