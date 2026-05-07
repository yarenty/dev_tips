---
title: Rust CLI
main_link: https://docs.rs/clap/latest/clap/
keywords: [cli, rust, clap, command-line, dialoguer, indicatif, prompts, progress]
status: reviewed
review_date: 2026/05/03
---

# Rust CLI

Building command-line tools in Rust. The canonical argument parser is **[clap](https://docs.rs/clap/latest/clap/)** (v4 in 2025); around it sit specialised crates for interactive prompts, progress bars, colour, and prettier error reporting. This section covers the *non-fullscreen* side of terminal UX — for retained-mode full-screen apps see [[../tui/README|Rust TUI]], and for built-in-Rust CLIs you'd use as an end user see [[../tooling/README|Rust tooling]].

## Articles

- [[dialoguer]] — Inquirer.js-style interactive prompts (Confirm/Input/Select/MultiSelect/Password/FuzzySelect).
- [[indicatif]] — progress bars, spinners, multi-progress, byte-formatting helpers.

## The Rust CLI ecosystem

### Argument parsing

| Crate | When to use |
|-------|-------------|
| [`clap`](https://docs.rs/clap/) | Default. Derive macro (`#[derive(Parser)]`), subcommands, shell completions, help text. The right answer 95% of the time. |
| [`argh`](https://crates.io/crates/argh) | Google's tiny clap alternative; very small binary footprint. |
| [`lexopt`](https://crates.io/crates/lexopt) | Hand-rolled parsing primitives — write your own POSIX-style loop. Good if `clap`'s derive feels too magical. |
| [`gumdrop`](https://crates.io/crates/gumdrop) | Older minimal alternative; mostly displaced by `argh`. |
| [`bpaf`](https://crates.io/crates/bpaf) | Functional/applicative parser combinator approach; powerful but niche. |

### Interactive prompts

| Crate | Notes |
|-------|-------|
| [[dialoguer]] | Most popular; from the `console-rs` family that also gives you [[indicatif]]. |
| [`inquire`](https://crates.io/crates/inquire) | Newer, often considered better-maintained and has nicer styling out of the box. |
| Bare `clap` | If you can avoid prompts entirely, do — non-interactive flags are scriptable. |

### Progress UI

| Crate | Notes |
|-------|-------|
| [[indicatif]] | Default. Single bars, spinners, `MultiProgress` for parallel jobs. |
| [`kdam`](https://crates.io/crates/kdam) | A Rust port of Python's `tqdm`; familiar API for ML/Python folks. |
| [`linya`](https://crates.io/crates/linya) | Lightweight multi-bar with a different concurrency model. |

### Colour & styling output

| Crate | Notes |
|-------|-------|
| [`owo-colors`](https://crates.io/crates/owo-colors) | Modern, ergonomic colour styling; often the default pick. |
| [`anstyle`](https://crates.io/crates/anstyle) | Foundation crate used by clap, ratatui-color, etc. — minimal styling primitives. |
| [`console`](https://crates.io/crates/console) | Sibling of dialoguer/indicatif; `style!`/`Term`/cursor primitives. |
| [`colored`](https://crates.io/crates/colored) | Older, still widely used. |
| [`yansi`](https://crates.io/crates/yansi) | Lightweight, zero-allocation alternative. |

### Error reporting

| Crate | Notes |
|-------|-------|
| [[../core/error|`anyhow` / `thiserror` / `eyre`]] | The error-handling triad — anyhow for app code, thiserror for libraries. |
| [`miette`](https://crates.io/crates/miette) | Pretty diagnostic output (think `rustc`'s nice error rendering) for CLIs that want to highlight source spans. |
| [`color-eyre`](https://crates.io/crates/color-eyre) | Drop-in `eyre` upgrade with colour and `Span` support. |

## Cross-section see-also

- [[../tui/README|Rust TUI]] — when you outgrow flag-based interaction and need a full-screen UI.
- [[../tooling/README|Rust tooling]] — curated index of CLIs *built in* Rust (bottom, ripgrep, bat, exa, …) you can use as design references.
- [[../core/error|Rust error handling]] — `anyhow` / `thiserror` / `eyre` / `miette` deep-dive.
- [[../../../tools/shell/README|tools/shell]] — generic shell tooling that often wraps Rust CLIs.

## Keywords

`#rust` `#cli` `#clap` `#dialoguer` `#indicatif` `#prompts` `#progress`
