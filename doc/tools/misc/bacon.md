---
title: "Bacon — background Rust code checker"
main_link: https://dystroy.org/bacon/
keywords: [bacon, rust, cargo-watch, background-checker, dystroy, tui, watch-mode]
status: reviewed
---

# Bacon — background Rust code checker

**Main link:** <https://dystroy.org/bacon/>

Repo: <https://github.com/Canop/bacon>

## Summary

Bacon is a background Rust code checker by Denys Séguret (Canop / dystroy.org — same author as `broot`). It runs `cargo check` (or `clippy`, `test`, `nextest`, `doc`, `run`, …) on file change and shows you the **errors first, then warnings**, in a small dedicated terminal window. It's designed for minimal interaction: launch it once in a side pane, code in your editor, and glance over when something breaks.

## Insight

This is the Rust dev-loop tool I install before anything else on a fresh machine. The `cargo check` cycle is fast enough that running it _per save_ in a side pane is realistic, and Bacon makes the output actually scannable:

- **Errors before warnings** — and first errors before last errors, so you don't scroll up.
- **Compact** — fits in a small split, leaves screen real estate for your editor.
- **Multiple jobs** (`bacon clippy`, `bacon test`, `bacon nextest`) — switch with a key.
- **`bacon-ls`** — there's a Language Server bridge that surfaces Bacon diagnostics in [[helix]] / VS Code / Neovim without needing rust-analyzer to recompile.

When to reach for it:

- You're in a Rust project, your editor is fast, and you want a continuous compile/test loop without `cargo watch`'s rough edges.
- You work in [[helix]], [[tools/linux/tmux|tmux]], or any setup where a side pane is natural.
- You pair it with [[lazygit]] (commit), [[prek]] (pre-commit gate), and rust-analyzer (in-editor) for a full workflow.

Comparisons:

- vs **`cargo watch`** — Bacon is more polished (better output formatting, jobs concept, key-driven controls); `cargo-watch` is simpler and integrates with non-cargo commands.
- vs **`cargo-nextest`** in watch mode — `nextest` is the test runner; Bacon can _drive_ it (`bacon nextest`).
- vs **rust-analyzer** — different jobs. RA gives you in-editor jump-to-def / hover; Bacon gives you the build/test result. Use both.

## Similar / related topics

- [`cargo-watch`](https://github.com/watchexec/cargo-watch) — the simpler ancestor.
- [`cargo-nextest`](https://nexte.st/) — fast Rust test runner; pairs with Bacon.
- [`broot`](https://dystroy.org/broot/) — same author; interactive directory navigator.
- [`watchexec`](https://watchexec.github.io/) — generic file-change runner.
- [[prek]] — gate at commit time; Bacon is the inner loop.

## Internal links

<!-- reviewed -->

- [[helix]] — pair with `bacon-ls` for inline diagnostics in Helix.
- [[tools/linux/tmux|tmux]] — keep Bacon in a persistent side pane.
- [[lazygit]] — Git side of the same dev loop.
- [[prek]] — commit-time gate; Bacon is the continuous checker.
- [[lstr]] — same dystroy/broot ecosystem flavour.

## Keywords

`#bacon` `#rust` `#cargo-watch` `#background-checker` `#dystroy` `#tui` `#watch-mode` `#misc` `#tools`

## References / raw notes

> # Bacon
>
> <https://dystroy.org/bacon/>
>
> bacon is a background code checker.
>
> It's designed for minimal interaction so that you can just let it run, alongside your editor, and be notified of warnings, errors, or test failures in your Rust code.
>
> It conveys the information you need even in a small terminal so that you can keep more screen estate for your other tasks.
>
> It shows you errors before warnings, and the first errors before the last ones, so you don't have to scroll up to find what's relevant.

Screenshot: <https://dystroy.org/bacon/img/vi-and-bacon.png>
