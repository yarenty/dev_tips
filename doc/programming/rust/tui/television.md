---
title: Television (tv)
main_link: https://github.com/alexpasmantier/television
keywords: [television, tv, rust, fuzzy, tui, ratatui, fzf, picker]
status: reviewed
review_date: 2026/05/03
---

# Television (`tv`)

**Main link:** <https://github.com/alexpasmantier/television>

## Summary

`television` (binary name `tv`, by Alex Pasmantier) is a fast, extensible fuzzy-finder TUI written in Rust on top of [[../tui/README|Ratatui]]. It searches arbitrary data sources — files, git repositories, environment variables, Docker images, shell history, command outputs — using a fuzzy matching algorithm, and is designed so you can plug in new sources via a simple config rather than hard-coding them. Think of it as `fzf`'s younger Rust-native cousin with first-class extensibility.

## Insight

Reach for `tv` if you like `fzf`'s general shape (fuzzy-pick from a list, get the result back) but want:

- A single static Rust binary (no awk/sed/realpath plumbing baked in).
- A config-file-driven model for declaring new sources/channels (instead of bash one-liners).
- Tight Ratatui-based UI that integrates well with other Rust terminal tooling.

It's still a small project compared to `fzf`'s decade-plus of integrations and editor plugins, so don't expect feature parity. If you already have `fzf` working in your shell/editor, the switching cost is real; pick `tv` for new tools or as part of an "all-Rust replacement" diet.

It's filed under `tui/` here because the *implementation* uses Ratatui, but as an end user you treat it like a CLI utility (it's also discussed in [[../tooling/README|tooling]] for that reason).

## Similar / related topics

- [`fzf`](https://github.com/junegunn/fzf) — the Go-based, decade-old standard for terminal fuzzy finding.
- [`sk` / skim](https://github.com/skim-rs/skim) — Rust fzf-clone, the original prior art.
- [`fzy`](https://github.com/jhawthorn/fzy) — minimal C fuzzy matcher; library focus.
- [[../tui/README|Ratatui]] — the framework `tv` is built on.

## Internal links
- [[../tui/README|Rust TUI]]
- [[../tooling/README|Rust tooling]] — sibling section for end-user CLIs/TUIs.
- [[../tooling/ripgrep|ripgrep]] — common pairing for "find files by content, then pick".

## Keywords

`#rust` `#tui` `#television` `#fuzzy-finder` `#ratatui` `#cli`

## References / raw notes

- Repo: <https://github.com/alexpasmantier/television>

> Television is a fast and versatile fuzzy finder TUI. It lets you quickly search through any kind of data source (files, git repositories, environment variables, docker images, you name it) using a fuzzy matching algorithm and is designed to be easily extensible.
