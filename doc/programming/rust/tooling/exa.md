---
title: exa — modern `ls` (unmaintained; use `eza` fork)
main_link: https://the.exa.website/
keywords: [exa, eza, rust, ls, unix-replacement, file-listing, cli]
status: reviewed
review_date: 2026/05/03
---

# exa — modern `ls` (unmaintained; use `eza` fork)

**Main link:** <https://the.exa.website/>

## Summary

`exa` is a Rust replacement for `ls(1)` by Benjamin Sago that adds colour-coding by file type, git-status columns, extended-attribute display, tree mode, and friendlier defaults than POSIX `ls` flags. **As of 2023 the upstream `exa` repo is unmaintained** (Benjamin stopped responding, no releases since 2022); the active community fork is [`eza`](https://github.com/eza-community/eza) at <https://github.com/eza-community/eza>. `eza` is a near-drop-in replacement, available in most package managers, and is what you should actually install in 2024+.

## Insight

Reach for `eza` (not `exa`) when you want a friendlier `ls` for interactive use — colour by file type and git status are the killer features. The flag set is also more sensible (`-l --git --header --classify` instead of cryptic short letters). **Don't alias `ls=eza`** in scripts: column output and option parsing differ enough that `awk`-pipe consumers will break. Common interactive aliases:

```sh
alias l='eza --icons --git'
alias ll='eza -l --icons --git --header'
alias lt='eza --tree --level=2 --icons'
```

For the `tree`-only niche, see [[../../../tools/design/lstr|lstr]] (lighter, interactive). For broader replace-the-coreutils background, see [[ripgrep]], [[bat]], [[dust]].

## Similar / related topics

- [`eza`](https://github.com/eza-community/eza) — **the active fork**; what to actually install.
- `ls(1)` — the POSIX original.
- [`lsd`](https://github.com/lsd-rs/lsd) — Rust competitor, more icon-heavy by default.
- [[../../../tools/design/lstr|lstr]] — fast minimalist Rust `tree` viewer with interactive mode.
- [`broot`](https://dystroy.org/broot/) — interactive directory navigator (heavier than tree-mode `eza`).

## Internal links

- [[README]] — tooling section landing.
- [[bat]] / [[ripgrep]] / [[dust]] — siblings in the "Rust replaces a Unix tool" family.
- [[../../../tools/shell/must_have|must_have]] — the "fresh box" CLI bundle.
- [[../../../tools/design/lstr|lstr]] — Rust tree viewer with overlapping niche.

## Keywords

`#exa` `#eza` `#rust` `#ls` `#unix-replacement` `#cli` `#file-listing`

## References / raw notes

- Original (unmaintained) site: <https://the.exa.website/>
- Original repo: <https://github.com/ogham/exa> (no commits since 2022)
- **Active fork:** <https://github.com/eza-community/eza>
- Crate (legacy): <https://crates.io/crates/exa>
