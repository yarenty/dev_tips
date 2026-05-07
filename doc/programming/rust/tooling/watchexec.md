---
title: watchexec — generic file-watch command runner
main_link: https://github.com/watchexec/watchexec
keywords: [watchexec, rust, file-watch, entr, fswatch, cargo-watch, nodemon]
status: reviewed
review_date: 2026/05/03
---

# watchexec — generic file-watch command runner

**Main link:** <https://github.com/watchexec/watchexec>

## Summary

`watchexec` is a Rust CLI by Félix Saparelli (passcod) that watches a directory tree and runs a command whenever files change. It's the generic, language-agnostic engine underneath `cargo-watch`, and is intentionally simple: monitor → debounce → kill (optional) → run. It works on Linux (inotify), macOS (FSEvents), and Windows; respects `.gitignore` and `.ignore`; coalesces editor-save bursts; uses process groups so subprocess trees get cleaned up.

## Insight

Reach for `watchexec` whenever you'd write `while inotifywait …; do …; done`. Common patterns:

```sh
# Re-run tests on any change
watchexec -e rs,toml -- cargo test

# Watch only src/, exclude target/
watchexec -w src --ignore-nothing -- cargo build

# Restart a long-running process on change (kill + restart)
watchexec -r -- cargo run --bin server

# Filter to specific extensions
watchexec --exts py,html -- python manage.py runserver

# Print the changed paths to env vars (for scripts)
watchexec --print-events -- ./on-change.sh
```

**`cargo-watch`** is a thin Rust-specific wrapper around `watchexec` (it just translates `cargo watch -x test` into `watchexec -e rs -- cargo test`). For Rust projects you can reach for either; `cargo-watch` has slightly nicer Rust defaults, `watchexec` is more general.

**Compared to siblings**:

- **`entr`** (C) — the venerable `find … | entr -r cmd` pattern; minimal, no recursion by default.
- **`fswatch`** (C++) — pure file-event emitter; you pipe to `xargs` yourself.
- **`nodemon`** (Node) — node-specific; pulls in npm if you don't already have it.
- **`reflex`** (Go) — similar; smaller community.
- **[[mprocs]]** — pair with watchexec to auto-restart specific processes within an mprocs setup.

**Gotchas**: kill-and-restart (`-r`) sends SIGTERM by default; some processes (Java, JVMs in general, anything that reaps signals into its own logic) may need `--signal SIGKILL`. Editor swap-files cause spurious events; use `--ignore` patterns. Filesystem events on network-mounted dirs can be flaky on every OS.

## Similar / related topics

- `cargo-watch` — Rust-specific thin wrapper.
- `entr` (C) — the classic `find | entr` pattern.
- `fswatch` (C++) — bare event emitter.
- `nodemon` (Node) — node-specific.
- `reflex` (Go) — Go-flavoured equivalent.

## Internal links

- [[README]] — tooling section landing.
- [[mprocs]] — pair to auto-restart processes inside an mprocs setup.
- [[../../../tools/misc/bacon|bacon]] — Rust-specific watch-mode for cargo + diagnostics.

## Keywords

`#watchexec` `#rust` `#file-watch` `#cargo-watch` `#entr` `#fswatch` `#dev-workflow`

## References / raw notes

- Repo: <https://github.com/watchexec/watchexec>
- Crate: <https://crates.io/crates/watchexec>
- Install: `cargo install watchexec-cli` or `brew install watchexec`.
- Companion crate: `cargo-watch` (Rust-specific wrapper).
