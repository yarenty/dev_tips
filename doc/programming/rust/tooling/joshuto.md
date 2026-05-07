---
title: joshuto — Ranger-style terminal file manager
main_link: https://github.com/kamiyaa/joshuto
keywords: [joshuto, rust, file-manager, ranger, tui, miller-columns]
status: reviewed
---

# joshuto — Ranger-style terminal file manager

**Main link:** <https://github.com/kamiyaa/joshuto>

## Summary

`joshuto` is a Rust terminal file manager by Jeff Zhao (kamiyaa) inspired by [Ranger](https://github.com/ranger/ranger) (Python): three-pane Miller columns showing parent / current / preview, vim-style keybindings, configurable via TOML, with image previews via `ueberzug` / Kitty / iTerm protocols when supported. The aim is "Ranger but fast and not Python".

## Insight

Reach for `joshuto` if you live in Ranger but want startup that doesn't cost a second of Python interpreter, or if you're on a small box where you don't want to install Python at all. The keybindings are deliberately Ranger-compatible (hjkl to move between columns, `gg/G/yy/dd/pp`), so muscle memory carries over. Configuration in `~/.config/joshuto/{joshuto,keymap,mimetype}.toml`.

**Compared to siblings** (the Rust TUI file-manager landscape is crowded):

- **`yazi`** — currently the most active and best-defaults; async architecture; image preview "just works"; the recommended starting point for new users.
- **`xplr`** — Lua-scriptable; smaller core, more flexible, steeper config curve.
- **`superfile`** (`spf`, Go) — modern aesthetic, file-tabs metaphor; covered at [[../../../tools/misc/superfile|superfile]].
- **`broot`** — not really a file manager but worth knowing; tree-view + fuzzy navigator.
- **`lf`** (Go) — Ranger-shaped, single binary, very fast.
- **`nnn`** (C) — minimal, plugin-driven.
- **`mc`** (C) — Midnight Commander; the venerable Norton-Commander-style two-panel.

If you're picking *today* with no inertia: try `yazi` first. Use `joshuto` if you specifically want Ranger semantics in Rust.

## Similar / related topics

- [Ranger](https://github.com/ranger/ranger) — the Python original this clones.
- [`yazi`](https://github.com/sxyazi/yazi) — currently the most-recommended Rust TUI file manager.
- [`xplr`](https://github.com/sayanarijit/xplr) — Lua-scriptable Rust file manager.
- [`lf`](https://github.com/gokcehan/lf) — Go file manager, Ranger-shaped.
- [[../../../tools/misc/superfile|superfile]] — modern Go file manager (different vibe).

## Internal links

- [[README]] — tooling section landing.
- [[../../../tools/misc/superfile|superfile]] — sibling file-manager article.
- [[../../../tools/design/lstr|lstr]] — fast `tree` viewer (overlapping interactive niche).

## Keywords

`#joshuto` `#rust` `#file-manager` `#ranger` `#tui` `#miller-columns` `#cli`

## References / raw notes

- Repo: <https://github.com/kamiyaa/joshuto>
- Inspired by Ranger; vim-style keybindings; TOML configuration.
- Install: `cargo install joshuto` or `brew install joshuto`.
