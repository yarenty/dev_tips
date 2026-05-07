---
title: xc — Markdown-defined task runner
main_link: https://github.com/joerdav/xc
keywords: [xc, task-runner, markdown, joerdav, make, just]
status: reviewed
review_date: 2026/05/03
---

# xc — Markdown-defined task runner

**Main link:** <https://github.com/joerdav/xc>

## Summary

`xc` is a task runner by Joe Davidson that reads its tasks straight out of a project's `README.md` (or any Markdown file). Each level-2 heading becomes a task name, and the fenced shell block under it becomes the task body — so the same document is your developer documentation *and* your runnable task list. There's first-class dependency declaration, env-var blocks, an interactive picker, and a single static binary written in Go (the "in Rust" filename in this vault is aspirational — see Insight).

## Insight

The pitch is "kill the README/Makefile drift": instead of a `README` that lists commands and a `Makefile` that runs them (which inevitably diverge), `xc` makes the README the source of truth. Compare to [just](https://github.com/casey/just) (justfile DSL, Rust), [task](https://taskfile.dev/) (YAML, Go), and `make` itself — `xc`'s differentiator is that the task definitions are *literally readable docs*. The "XC in Rust" filename in this vault was originally a TODO note ("port xc to Rust as a learning project"); xc itself is Go. Reach for it on small/medium projects where you also write a project README; on bigger setups with non-trivial dependency graphs, `just` or `make` are still better choices.

## Similar / related topics

- [just](https://github.com/casey/just) — Rust task runner with its own justfile DSL; the closest competitor.
- [task](https://taskfile.dev/) — YAML-defined task runner in Go.
- [make](https://www.gnu.org/software/make/) — the original; still unbeaten for build graphs.
- [mask](https://github.com/jacobdeichert/mask) — also Markdown-defined tasks, Rust, slightly older project.
- [`cargo-make`](https://github.com/sagiegurari/cargo-make) — Rust-ecosystem task runner with TOML config.

## Internal links
- [[_todo_ideas]]
- [[from_easy_to_advanced]]
- [[README]]

## Keywords

`#xc` `#task-runner` `#markdown` `#joerdav` `#make` `#just`

## References / raw notes

- Repo: <https://github.com/joerdav/xc>
- Docs: <https://xcfile.dev/>
- The xc README is its own task list — read it for an executable example.
- Possible learning project: re-implement a small `xc`-compatible parser-runner in Rust as practice with `pulldown-cmark` (Markdown parsing) + `tokio::process` (subprocess management).
