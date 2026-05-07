---
title: bat — `cat` clone with syntax highlighting
main_link: https://github.com/sharkdp/bat
keywords: [bat, rust, cat, syntax-highlighting, less, pager, unix-replacement]
status: reviewed
---

# bat — `cat` clone with syntax highlighting

**Main link:** <https://github.com/sharkdp/bat>

## Summary

`bat` is a `cat(1)` clone written in Rust by David Peter (sharkdp) that adds syntax highlighting (via the `syntect` crate / Sublime Text grammars), git-diff gutter markers, automatic paging, line numbers, and non-printable-character display. It auto-detects whether output is a TTY and falls back to plain `cat` semantics when piped, so you can drop it in as an alias without breaking scripts. Themes and grammars are pluggable via `~/.config/bat/`, and `bat --diff` integrates with `git`.

## Insight

Reach for `bat` whenever you'd `cat` a source file or config — the highlighting alone earns it. The auto-paging is the second killer feature: `bat src/main.rs` paginates if the file is bigger than the terminal, plain-prints otherwise. **Don't alias `cat=bat`** — scripts and pipes that depend on POSIX `cat` semantics will silently break (it's mostly compatible, but not 100%, especially around escape sequences and trailing newlines). Common pairing: `MANPAGER="sh -c 'col -bx | bat -l man -p'"` to colourise man pages; `git config --global core.pager "bat --paging=always"` for coloured `git show` / `git log -p`.

Compared to siblings: **`pygmentize` / `ccat`** are the older Python/Go equivalents, slower and uglier; **`riff`** (also Rust) does syntax-aware *diff* highlighting (closer to `[[difftastic]]`) and isn't a `cat` replacement; **`bat-extras`** ships convenience wrappers (`batgrep`, `batman`, `prettybat`).

## Similar / related topics

- `cat`(1) — the POSIX original; bat falls back to it when piped.
- `pygmentize` (Python) / `ccat` (Go) — older syntax-highlighting `cat`s.
- [[difftastic]] / `riff` — syntax-aware diff tools (related but different niche).
- [`delta`](https://github.com/dandavison/delta) — Rust pager for `git diff` specifically; pairs nicely with bat as the `core.pager`.
- [[ripgrep]], [[exa]], [[dust]] — siblings in the "Rust replaces a Unix tool" family.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[ripgrep]] — the `grep` sibling.
- [[exa]] — the `ls` sibling.
- [[difftastic]] — syntax-aware diff (different niche).
- [[../../../tools/shell/must_have|must_have]] — the "fresh box" CLI bundle that includes `bat`.

## Keywords

`#bat` `#rust` `#cat` `#syntax-highlighting` `#pager` `#unix-replacement` `#cli`

## References / raw notes

- Repo: <https://github.com/sharkdp/bat>
- Crate: <https://crates.io/crates/bat>
- A `cat(1)` clone with wings — syntax highlighting, git diff integration, automatic paging.
