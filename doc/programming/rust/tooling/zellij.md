---
title: zellij — modern terminal multiplexer (tmux replacement)
main_link: https://zellij.dev/
keywords: [zellij, rust, terminal-multiplexer, tmux, screen, layouts, plugins, wasm]
status: reviewed
review_date: 2026/05/03
---

# zellij — modern terminal multiplexer (tmux replacement)

**Main link:** <https://zellij.dev/>

## Summary

`zellij` is a Rust terminal multiplexer / workspace by Aram Drevekenin (also of [[bandwhich]]). It does the same job as `tmux`/`screen` — split the terminal into panes/tabs, persist sessions across SSH disconnects — but with discoverable defaults: a status bar with all keybindings always visible, mouse support, layouts described in KDL, floating panes, and a Wasm plugin system. Sessions are detached and re-attached with `zellij attach`.

> Filename ambiguity: `zellij.md` exists at both `programming/rust/tooling/zellij.md` (this — built-with-Rust angle) and [[../../../tools/linux/zellij|tools/linux/zellij]] (the user-facing tools angle). Both are linked.

## Insight

Reach for `zellij` if `tmux` has always felt user-hostile (the leader key, no on-screen affordances, plugin ecosystem in plain bash) and you want the same persistence model with modern UX. The killer features are:

1. **Always-visible keybinding hint bar** at the bottom — no more `man tmux`-driven exploration.
2. **Layouts as data** (KDL files) — `zellij --layout dev.kdl` opens a pre-shaped workspace.
3. **Floating panes** — useful for ephemeral tools (a quick lazygit) without disrupting the main layout.
4. **Wasm plugins** — write status-bar widgets / pickers in any language that compiles to Wasm.

**Compared to `tmux`**: zellij is friendlier on day one but less battle-tested at extreme scale (hundreds of nested sessions, exotic terminal types). `tmux`'s plugin ecosystem (TPM, tmux-resurrect, tmux-continuum) is still richer. **Cross-link**: the broader tmux deep-dive lives at [[../../../tools/linux/tmux|tools/linux/tmux]] (config, plugins, ~/.tmux.conf starter, vs screen).

**Compared to `mprocs`**: [[mprocs]] is **not a multiplexer** — it's a fixed multi-process orchestrator (more like a Procfile runner with a TUI). Use `zellij` for general terminal work; use `mprocs` when you have N specific long-running commands (DB, server, log tail) that you always want side-by-side.

**Gotchas**: the leader key (`Ctrl+g` for unlock-mode by default) differs from tmux's `Ctrl+b`; `zellij setup --dump-config` writes a starter config. Old SSH targets with no UTF-8 terminal will look broken.

## Similar / related topics

- `tmux` — the dominant incumbent; richer plugin ecosystem.
- `screen` — the venerable original GNU multiplexer.
- `byobu` — opinionated tmux/screen wrapper with better defaults.
- [[mprocs]] — fixed multi-process TUI orchestrator (different model).
- [[../../../tools/linux/tmux|tools/linux/tmux]] — tmux deep-dive (cross-section sibling).

## Internal links

- [[README]] — tooling section landing.
- [[mprocs]] — sibling process-orchestration tool (different niche).
- [[../../../tools/linux/tmux|tools/linux/tmux]] — tmux deep-dive (the thing zellij replaces).
- [[bandwhich]] — same author.

## Keywords

`#zellij` `#rust` `#terminal-multiplexer` `#tmux` `#layouts` `#plugins` `#wasm` `#tui`

## References / raw notes

- Site: <https://zellij.dev/>
- Repo: <https://github.com/zellij-org/zellij>
- Install: `cargo install --locked zellij` or `brew install zellij`.

```sh
cargo install --locked zellij
zellij                          # start a new session
zellij list-sessions
zellij attach <name>
zellij --layout ~/dev.kdl       # pre-shaped workspace
```
