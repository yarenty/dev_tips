---
title: bottom (`btm`) — `top`/`htop` replacement
main_link: https://github.com/ClementTsang/bottom
keywords: [bottom, btm, rust, top, htop, system-monitor, tui, unix-replacement]
status: reviewed
---

# bottom (`btm`) — `top`/`htop` replacement

**Main link:** <https://github.com/ClementTsang/bottom>

## Summary

`bottom` (binary `btm`) is a customisable cross-platform graphical process / system monitor for the terminal, by Clement Tsang. It runs on Linux, macOS, Windows, and FreeBSD; shows CPU / memory / network / disk / temperature / battery / process panels with mouse-and-keyboard navigation, sortable columns, kill, search, and per-widget configuration. The TUI is built on `tui-rs` / Ratatui and the binary name `btm` was chosen because `bottom` was already taken on most distros.

## Insight

Reach for `btm` when you want one tool that gives you `top` + `iotop` + a memory chart + a network graph + a process tree, with sane colours, on any OS, without learning a different keymap per platform. The killer features compared to `htop` are (a) **the live graphs** — CPU and net history, not just current values — and (b) **cross-platform**: same binary, same UI, on a Mac and a Linux server. Configuration lives at `~/.config/bottom/bottom.toml` and lets you reorganise widgets, set update intervals, theme colours, and define custom layouts.

**Gotchas**: temperature sensors require `lm_sensors` on Linux and may be empty in a container; on macOS, GPU info is partial; the `--battery` flag is opt-in. Compared to siblings: **`top`** is universal but ugly; **`htop`** is the comfortable middle (C, no graphs); **`glances`** (Python) shows more out of the box but is slower; **`bashtop`/`bpytop`/`btop`** are the *btop family* — heavier graphics, rounded boxes, more themes. Pick `btm` if you want fast + Rust + cross-platform; pick `btop` if you want eye-candy.

## Similar / related topics

- `top`(1) / `htop` — the classic terminal process monitors.
- [`btop`](https://github.com/aristocratos/btop) — the heavier C++ competitor with rounded-box aesthetics.
- `glances` — Python-based all-in-one system monitor.
- [[bandwhich]] — bandwidth-by-process top (different niche, complementary).
- [[../../../observability/node_exporter|node_exporter]] — for *exporting* host metrics rather than viewing them.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[bandwhich]] — sibling network-only top.
- [[bat]] / [[ripgrep]] / [[exa]] — siblings in the "Rust replaces a Unix tool" family.
- [[../../../observability/node_exporter|node_exporter]] — for Prometheus-scrapable host metrics.

## Keywords

`#bottom` `#btm` `#rust` `#top` `#htop` `#system-monitor` `#tui` `#unix-replacement`

## References / raw notes

- Repo: <https://github.com/ClementTsang/bottom>
- Crate: <https://crates.io/crates/bottom>
- Cross-platform: Linux / macOS / Windows / FreeBSD.
- Install: `cargo install bottom` (binary is `btm`), or `brew install bottom`.
