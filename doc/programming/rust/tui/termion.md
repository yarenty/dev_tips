---
title: Termion
main_link: https://gitlab.redox-os.org/redox-os/termion
keywords: [termion, rust, redox, tty, terminal, ansi, low-level]
status: reviewed
---

# Termion

**Main link:** <https://gitlab.redox-os.org/redox-os/termion>

## Summary

Termion is a pure-Rust, dependency-free library for low-level terminal manipulation: enter raw mode, move the cursor, read keypresses, emit ANSI colour/style escapes, query terminal size. It was originally built for Redox OS but works on Redox, macOS, BSD, and Linux (anywhere with an ANSI-compatible TTY) — but **not Windows**. There are no `ncurses`/`termbox` bindings; Termion talks directly to the TTY and generates the escape sequences itself.

## Insight

Reach for Termion when you want raw terminal control without a higher-level toolkit — small CLIs that need a single overwriting status line, password prompts, ad-hoc TUIs, or custom backends for higher-level libraries (Cursive can sit on top of Termion, and inquirer-rs uses it). The whole API is a thin layer over ANSI escapes, so reading the source is a fast way to learn what the codes actually do.

In 2025, the practical default for *new* projects is **`crossterm`**, not Termion:

- `crossterm` is **cross-platform** (works on Windows, where Termion does not).
- `crossterm` is what Ratatui uses by default.
- `console` (from the [console-rs](https://github.com/console-rs) family — same group as `dialoguer`/`indicatif`) is another popular cross-platform alternative aimed at simpler use cases.

Termion is still useful if you want a tiny pure-Rust, Unix-only dependency, or you're working in the Redox ecosystem, or you're reading existing code that uses it.

## Similar / related topics

- [crossterm](https://crates.io/crates/crossterm) — cross-platform replacement; the modern default.
- [`console`](https://crates.io/crates/console) — colour/style/cursor primitives in the console-rs family.
- `termwiz` — wezterm's terminal-handling crate, also cross-platform.
- [[../tui/README|Ratatui]] — high-level TUI built *on top of* one of these backends.
- [[cursive]] — high-level toolkit; can use Termion as a backend.

## Internal links
<!-- reviewed -->
- [[../tui/README|Rust TUI]]
- [[cursive]]
- [[iocraft]]
- [[../net/paho_mqtt|paho_mqtt]] — example of a long-running CLI that benefits from raw TTY control.

## Keywords

`#rust` `#tui` `#termion` `#ansi` `#tty` `#redox` `#low-level`

## References / raw notes

- Repo: <https://gitlab.redox-os.org/redox-os/termion>
- Crate: <https://crates.io/crates/termion>

> Termion is a pure Rust, bindless library for low-level handling, manipulating and reading information about terminals. This provides a full-featured alternative to Termbox.
>
> Termion aims to be simple and yet expressive. It is bindless, meaning that it is not a front-end to some other library (e.g., ncurses or termbox), but a standalone library directly talking to the TTY. Termion is quite convenient, due to its complete coverage of essential TTY features, providing one consistent API. Termion is rather low-level containing only abstraction aligned with what actually happens behind the scenes. For something more high-level, refer to inquirer-rs, which uses Termion as backend.
>
> Termion generates escapes and API calls for the user. This makes it a whole lot cleaner to use escapes.
>
> Supports Redox, Mac OS X, BSD, and Linux (or, in general, ANSI terminals).
