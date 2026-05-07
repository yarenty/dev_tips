---
title: clido — minimal CLI to-do list
main_link: https://github.com/Zij-IT/clido
keywords: [clido, rust, todo, cli, productivity]
status: reviewed
review_date: 2026/05/03
---

# clido — minimal CLI to-do list

**Main link:** <https://github.com/Zij-IT/clido>

## Summary

`clido` is a small Rust CLI to-do list by Zij-IT. It's a hobby-grade single-binary alternative to `taskwarrior`/`todo.txt-cli`, suitable for "I need three commands to add, list, complete TODOs from the shell". Storage is a local file; commands are typical (`clido add`, `clido list`, `clido done`).

## Insight

Reach for `clido` only if you specifically want a minimal Rust binary for shell-driven TODOs and don't want to set up Taskwarrior. For anything beyond that — recurrence, contexts, sync, mobile — pick one of the established players:

- **[Taskwarrior](https://taskwarrior.org/)** (`task`, C++) — the canonical CLI to-do list; rich filtering, contexts, recurrence, server sync. The right answer for serious shell-TODO use.
- **[`todo.txt-cli`](https://github.com/todotxt/todo.txt-cli)** — the spec + reference implementation around the famously simple `todo.txt` plain-text format.
- **`org-mode`** — if you live in Emacs.
- **[[../../../tools/misc/weektodo|weektodo]]** — desktop GUI for week-shaped planning.

If you want a Rust *productivity* TUI rather than a CLI list, look at [[../../../tools/misc/superfile|superfile]] (file mgmt) or `taskwarrior-tui`. `clido` is a fine 80-line example to learn `clap`/serde from but not what to invest a multi-year TODO history into.

## Similar / related topics

- [Taskwarrior](https://taskwarrior.org/) — the established CLI to-do list.
- [`todo.txt-cli`](https://github.com/todotxt/todo.txt-cli) — plain-text format + CLI.
- [[../../../tools/misc/weektodo|weektodo]] — desktop GUI alternative.
- `org-mode` — Emacs-native equivalent.

## Internal links

- [[README]] — tooling section landing.
- [[../../../tools/misc/weektodo|weektodo]] — desktop GUI sibling.

## Keywords

`#clido` `#rust` `#todo` `#cli` `#productivity`

## References / raw notes

- Repo: <https://github.com/Zij-IT/clido>
- A small TODO list in CLI by Zij-IT.
