---
title: Rust TUI
main_link: https://ratatui.rs/
keywords: [tui, rust, ratatui, cursive, termion, terminal, ncurses]
status: reviewed
---

# Rust TUI

Full-screen, redraw-the-whole-terminal-on-every-frame applications written in Rust. The de-facto modern default is **[Ratatui](https://ratatui.rs/)** (the actively-maintained 2023 community fork of the original `tui-rs`, which was archived by its author). Around it lives a small ecosystem of widget toolkits, low-level terminal-control libraries, and a few experimental declarative wrappers.

This is the *full-screen* corner of the terminal. For non-fullscreen ergonomics — argument parsing, prompts, progress bars — see [[../cli/README|Rust CLI]]. For desktop windows see [[../gui/README|Rust GUI]]. For curated standalone CLIs/TUIs written *in* Rust see [[../tooling/README|Rust tooling]].

## Articles

- [[cursive]] — high-level retained-mode widget toolkit (Dialog/EditView/Mux/…); the easiest first TUI.
- [[termion]] — low-level pure-Rust terminal control (raw mode, cursor, ANSI escapes); what you reach for *under* a higher-level lib, or for tiny tools.
- [[iocraft]] — React-style declarative TUI using Rust macros to mimic JSX.
- [[television]] — `tv`, a fuzzy launcher / picker built *on* Ratatui (think `fzf` competitor).
- [[ink]] — **JavaScript** (vadimdemedes/ink), filed here as the design ancestor that iocraft borrows from.

## Decision table

| You want… | Reach for |
|-----------|-----------|
| The default modern Rust TUI, used by Atuin, gitui, bottom, helix-style projects | **[Ratatui](https://ratatui.rs/)** |
| "Give me Dialog/EditView/MenuBar without writing a draw loop" | [[cursive]] |
| Raw mode + cursor moves + ANSI escapes only, no widgets | [[termion]] (or [`crossterm`](https://crates.io/crates/crossterm), which is what Ratatui actually uses under the hood) |
| Declarative JSX-style component tree | [[iocraft]] |
| A fuzzy picker as an end-user tool | [[television]] (`tv`) — or `fzf`/`sk`/`fzy` if you don't need Rust |
| Cross-platform terminal backend for Ratatui | [`crossterm`](https://crates.io/crates/crossterm) (the default), `termion` (Unix-only), or `termwiz` (wezterm's) |

## The tui-rs → Ratatui story

The original `tui-rs` crate (by Florian Dehau) was the standard for several years and powered Atuin, gitui, bottom, and many others. In early 2023 the maintainer stopped responding to PRs and the crate was archived. A community fork named **Ratatui** was created with the explicit goal of continuing development; within a few months it had absorbed most of the tui-rs ecosystem, picked up a docs site ([ratatui.rs](https://ratatui.rs/)), a book, and a steady release cadence. New code should always reach for Ratatui; `tui-rs` is dead.

## Cross-section see-also

- [[../cli/README|Rust CLI]] — argument parsing, prompts, progress bars (often used in the same binary).
- [[../gui/README|Rust GUI]] — when "fullscreen terminal" isn't enough and you need an actual window.
- [[../tooling/README|Rust tooling]] — many entries there (bottom, gitui, joshuto, helix-companions) are TUIs.
- [[../../../tools/linux/tmux|tmux]] — terminal multiplexer that often hosts these apps.

## Keywords

`#rust` `#tui` `#ratatui` `#cursive` `#termion` `#crossterm` `#terminal`
