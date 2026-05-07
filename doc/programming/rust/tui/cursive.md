---
title: Cursive
main_link: https://github.com/gyscos/cursive
keywords: [cursive, rust, tui, ncurses, widgets, gyscos]
status: reviewed
---

# Cursive

**Main link:** <https://github.com/gyscos/cursive>

## Summary

Cursive (by `gyscos`) is a high-level retained-mode TUI library for Rust. Where Ratatui hands you a frame buffer and asks you to redraw it every tick, Cursive gives you ready-made widgets — `Dialog`, `EditView`, `SelectView`, `LinearLayout`, `MenuBar` — and an event loop that calls *back* into your code. It defaults to an ncurses backend but also supports `termion`, `crossterm`, `pancurses`, and even a Wasm/web backend. There's a small but rich ecosystem of third-party views: tables, trees, tabs, mux, async/spinner overlays, hex viewers, HTML markup.

## Insight

Reach for Cursive when you want a *form-shaped* TUI — dialogs, menus, selects, edit fields — and don't want to hand-roll layout, focus, or input dispatch. It's the closest thing in Rust to "give me a Tk/wxWidgets but in the terminal". Trade-offs vs [[../tui/README|Ratatui]]:

- **Cursive is higher-level**, so simple apps are dramatically less code, but you give up control over the draw loop and per-frame layout.
- **Ratatui is the more popular choice** in 2025 — most modern terminal apps you'll point at as inspiration (gitui, atuin, bottom, scooter, …) are Ratatui. Cursive is more niche.
- The default ncurses backend brings a `libncursesw` system dependency on Linux/macOS; if you want pure-Rust, switch the backend feature flag to `crossterm` or `termion`.

The third-party-views story is unusually rich for a Rust TUI lib: `cursive_table_view`, `cursive_tree_view`, `cursive-tabs`, `cursive-multiplex` (tmux-style tiling), `cursive-async-view` (loading screens), `cursive-aligned-view`, `cursive_hexview`, `cursive-markup` (renders HTML inside a view). If your app fits any of those shapes, Cursive may save you weeks.

## Similar / related topics

- [[../tui/README|Ratatui]] — the immediate-mode, low-level alternative; far more popular in 2025.
- [[termion]] / [crossterm](https://crates.io/crates/crossterm) — backend layers Cursive can sit on top of.
- [[iocraft]] — declarative React-like alternative.
- [`dialoguer`](../cli/dialoguer.md) — when all you need is a couple of prompts, not a full screen.
- [`tview`](https://github.com/rivo/tview) — same shape, but in Go.

## Internal links
- [[../tui/README|Rust TUI]]
- [[termion]]
- [[iocraft]]
- [[../cli/dialoguer|dialoguer]]
- [[../tooling/bottom|bottom]] — example real-world Ratatui app to compare against.

## Keywords

`#rust` `#tui` `#cursive` `#ncurses` `#widgets` `#retained-mode`

## References / raw notes

- Crate: <https://crates.io/crates/cursive>
- Repo: <https://github.com/gyscos/cursive>

> A TUI (Text User Interface) library focused on ease-of-use. Cursive is a TUI library for rust. It uses ncurses by default, but other backends are available. It allows you to build rich user interfaces for terminal applications.

### Cursive ecosystem (third-party views)

- **Table view** — basic table widget — <https://github.com/BonsaiDen/cursive_table_view> · <https://crates.io/crates/cursive_table_view>
- **Tree view** — basic tree widget — <https://github.com/BonsaiDen/cursive_tree_view> · <https://crates.io/crates/cursive_tree_view>
- **Tabs** — wrapper view for switchable tabs — <https://github.com/deinstapel/cursive-tabs>
- **Spinner view** — animated spinner; `cargo run --example spinner` — <https://github.com/otov4its/cursive-spinner-view/> · <https://crates.io/crates/cursive-spinner-view>
- **Flexi-logger view** — debug view that respects `RUST_LOG` and `flexi_logger` config; auto-updates from file — <https://github.com/deinstapel/cursive-flexi-logger-view> · <https://crates.io/crates/cursive-flexi-logger-view>
- **Multiplex (tmux-style)** — tiling window manager inside Cursive — <https://github.com/deinstapel/cursive-multiplex> · <https://crates.io/crates/cursive-multiplex>
- **Async view** — loading screen wrapper for slow-to-construct or network-backed views — <https://github.com/deinstapel/cursive-async-view>
- **Align view** — align child views (center/left/right/top/bottom) — <https://github.com/deinstapel/cursive-aligned-view>
- **Hex view** — simple hex dumper — <https://github.com/hellow554/cursive_hexview>
- **Markup** — `cursive-markup` renders HTML inside a view — <https://crates.io/crates/cursive-markup> · <https://git.sr.ht/~ireas/cursive-markup-rs>

### Real-world example

- [Grin](https://github.com/mimblewimble/grin) — the Mimblewimble cryptocurrency node — has a substantial Cursive-based TUI worth studying as a real-world reference.
