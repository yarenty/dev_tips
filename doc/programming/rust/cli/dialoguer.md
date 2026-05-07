---
title: dialoguer
main_link: https://github.com/console-rs/dialoguer
keywords: [dialoguer, rust, cli, prompts, console-rs, inquirer]
status: reviewed
review_date: 2026/05/03
---

# dialoguer

**Main link:** <https://github.com/console-rs/dialoguer>

## Summary

`dialoguer` is the Rust answer to JavaScript's [Inquirer.js](https://github.com/SBoudrias/Inquirer.js) — a small library of interactive command-line prompts: `Confirm`, `Input`, `Password`, `Select`, `MultiSelect`, `Sort`, `FuzzySelect`, `Editor`. It's part of the `console-rs` family of crates (also home to [[indicatif]] and `console`), so it composes well with progress bars and styled output. Built on top of `console`, it works on Windows, macOS, and Linux without bringing in `ncurses` or a full TUI framework.

## Insight

Reach for `dialoguer` whenever a `clap` flag isn't enough — you need to ask the user something at runtime: "are you sure?", "pick one of these", "type your password". Don't reach for it when scriptability matters (CI, automation): every interactive prompt is a place a script will hang waiting for input. Pair it with `clap` + a `--yes`/`--non-interactive` flag pattern so users can skip prompts.

Practical notes:

- `FuzzySelect` is a hidden gem — fuzzy-filtered selection list, much nicer than scrolling a long `Select`.
- `Password` correctly hides input (uses `console::Term`'s raw mode).
- `Editor::new().edit()` opens `$EDITOR` and returns the edited string — fantastic for "give me a multi-line input" without trying to build a TUI text widget.
- The styling defaults are decent; for branded output, set themes via `ColorfulTheme::default()` and tweak.
- `Input` does in-process validation if you supply `.validate_with()`, so you don't have to `loop { prompt; check; reprompt }`.

The most-cited modern alternative is **[`inquire`](https://crates.io/crates/inquire)** — generally considered better-maintained in 2025 with nicer defaults; worth comparing for new projects.

## Similar / related topics

- [`inquire`](https://crates.io/crates/inquire) — newer, more actively maintained Rust prompt library.
- [`requestty`](https://crates.io/crates/requestty) — another Inquirer-style prompt lib for Rust.
- [Inquirer.js](https://github.com/SBoudrias/Inquirer.js) — the JavaScript original.
- [[indicatif]] — sibling crate from `console-rs`; pair them for "ask user → run with progress".
- [`clap`](https://docs.rs/clap/) — for non-interactive flags; the right answer when `dialoguer` would force scripts to hang.

## Internal links
- [[../cli/README|Rust CLI]]
- [[indicatif]]
- [[../tui/README|Rust TUI]] — when prompts aren't enough.

## Keywords

`#rust` `#cli` `#dialoguer` `#prompts` `#console-rs` `#interactive`

## References / raw notes

- Repo: <https://github.com/console-rs/dialoguer>
- Examples: <https://github.com/console-rs/dialoguer/tree/master/examples>
- Crate: <https://crates.io/crates/dialoguer>

> A rust library for command line prompts and similar things.
>
> Best paired with other libraries in the family: `console`, `indicatif`.
