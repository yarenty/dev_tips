---
title: "lstr — fast minimalist Rust tree viewer"
main_link: https://github.com/bgreenwell/lstr
keywords: [lstr, tree, ls, rust, cli, directory-viewer, interactive-tui]
status: reviewed
---

# lstr — fast minimalist Rust tree viewer

**Main link:** <https://github.com/bgreenwell/lstr>

## Summary

`lstr` is a blazingly fast, minimalist directory-tree viewer written in Rust by Brandon Greenwell. It does what `tree(1)` does — print a directory hierarchy with depth, filtering, and gitignore awareness — and adds an **interactive mode** (TUI) where you can navigate, expand/collapse, and act on entries with the keyboard.

## Insight

If you live in `tree`, `lstr` is the modern replacement: same mental model, faster, prettier, and the interactive mode is the killer feature. If you live in `eza` (the modern `ls`), `lstr` is the complementary tool: `eza --tree` is fine for a quick look; `lstr interactive` is what you want when you need to _explore_.

Reach for it when:

- You want a one-shot tree dump for a README or a bug report — pipe it.
- You're orienting yourself in an unfamiliar codebase and `find . -type d` isn't enough.
- You want a lightweight alternative to `broot` / `yazi` without leaving your shell prompt.

Comparisons:

- vs **`tree`** — same output, faster, gitignore-aware by default, plus interactive mode.
- vs **`eza --tree`** — `eza` is a fuller `ls` replacement; `lstr` focuses on the tree view.
- vs **`broot`** — `broot` is a heavier interactive file navigator with fuzzy search and previews; `lstr` is leaner.
- vs **`fd`** — different job; `fd` finds, `lstr` displays.

```bash
git clone https://github.com/bgreenwell/lstr.git
cd lstr
cargo install --path .

lstr [OPTIONS] [PATH]
lstr interactive [OPTIONS] [PATH]
```

## Similar / related topics

- [`tree`](http://mama.indstate.edu/users/ice/tree/) — the original.
- [`eza`](https://github.com/eza-community/eza) — modern `ls` (drop-in `tree` mode).
- [`broot`](https://dystroy.org/broot/) — interactive directory navigator, same author as `bacon`.
- [`yazi`](https://yazi-rs.github.io/) — image-previewing TUI file manager.
- [[superfile]] — multi-panel TUI file manager.

## Internal links

<!-- reviewed -->

- [[superfile]] — go from "look" to "do" with a real TUI file manager.
- [[czkawka]] — find what to clean up after `lstr` shows you the shape.
- [[bacon]] — same author/ecosystem flavour: small focused Rust CLI.

## Keywords

`#lstr` `#tree` `#ls` `#rust` `#cli` `#directory-viewer` `#interactive-tui` `#design` `#tools`

## References / raw notes

> A blazingly fast, minimalist directory tree viewer, written in Rust. Inspired by the command-line program `tree`, with a powerful interactive mode.

```bash
git clone https://github.com/bgreenwell/lstr.git
cd lstr
cargo install --path .

lstr [OPTIONS] [PATH]
lstr interactive [OPTIONS] [PATH]
```

Demo: <https://github.com/bgreenwell/lstr/raw/main/assets/lstr-demo.gif>
