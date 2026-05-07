---
title: dust — `du` replacement with tree visualisation
main_link: https://github.com/bootandy/dust
keywords: [dust, du-dust, rust, du, disk-usage, unix-replacement, cli]
status: reviewed
---

# dust — `du` replacement with tree visualisation

**Main link:** <https://github.com/bootandy/dust>

## Summary

`dust` (crate name `du-dust`, binary `dust`) is a Rust replacement for `du(1)` by Andy Boot that shows disk usage as an inline ASCII bar chart of the largest sub-trees, sorted descending, with sensible colour and unit defaults. The killer feature is that you can see *at a glance* where the bytes are going without piping through `sort -h | head`. Install with `cargo install du-dust` (the crate is `du-dust` to disambiguate from a Solidity-related `dust` crate, but the binary is `dust`) or via Homebrew/apt/dnf.

## Insight

Reach for `dust` whenever you'd reach for `du -sh */ | sort -h` to find what's eating your disk. The default invocation `dust ~/Downloads` gives an immediately useful tree with bar charts; `dust -d 1` limits depth, `dust -n 30` shows top 30, `dust -X .git` excludes patterns. **Gotcha**: the crate is `du-dust`, not `dust` — `cargo install dust` installs an unrelated crate. The binary is also commonly typo'd as "dusk" in older notes (this article was originally filed as `dusk_replacement_of_du.md`).

Compared to siblings: **`du`** is universal but you always have to compose it with `sort` and `head`; **`ncdu`** (C/Zig) is the classic interactive TUI — heavier but lets you delete in-place; **`gdu`** (Go) is faster than ncdu on huge trees and parallelises better; **`dua`** (Rust) is dust's closest competitor — also has an interactive mode. Pick `dust` for one-shot "where are my bytes?", pick `ncdu`/`dua` for "let me browse and delete".

## Similar / related topics

- `du(1)` — the POSIX original.
- [`ncdu`](https://dev.yorhel.nl/ncdu) — interactive curses disk-usage analyser (C / Zig rewrite).
- [`gdu`](https://github.com/dundee/gdu) — Go-based, fast on huge trees.
- [`dua`](https://github.com/Byron/dua-cli) — Rust competitor with interactive mode.
- [`duf`](https://github.com/muesli/duf) — `df` replacement (different tool, often paired).

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[bat]] / [[exa]] / [[ripgrep]] / [[bottom]] — siblings in the "Rust replaces a Unix tool" family.
- [[../../../tools/shell/must_have|must_have]] — the broader CLI-bundle reference.

## Keywords

`#dust` `#du-dust` `#rust` `#du` `#disk-usage` `#unix-replacement` `#cli`

## References / raw notes

- Repo: <https://github.com/bootandy/dust>
- Crate: <https://crates.io/crates/du-dust> (binary name: `dust`)
- Install:

  ```sh
  cargo install du-dust
  # or
  brew install dust
  ```

- Original notes (typo'd "dusk" → actual tool is `dust`):

  ```sh
  cargo install du-dust   # was: cargo install du-dusk (typo)
  dust
  ```
