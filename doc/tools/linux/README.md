---
title: Linux command-line tools
keywords: [linux, terminal, multiplexer, shell, editor, debugger, networking, books]
status: reviewed
review_date: 2026/05/03
---

# Linux command-line tools

A curated index of the terminal-centric tools collected in this section: multiplexers, shells, editors, git UIs, network tooling, debuggers, plus a small reading list. The articles here are opinionated — each one tries to say *when to reach for the tool* and what it competes with, not just "here is a link".

## Terminal multiplexers

- [[tools/linux/tmux|tmux]] — the venerable POSIX-friendly multiplexer; the baseline everyone else is measured against.
- [[zellij]] — modern Rust multiplexer with floating panes, a discoverable status bar, and sane defaults out of the box.
- [[byobu]] — a configuration layer over tmux/screen that gives you status bars, F-key bindings, and Ubuntu-friendly defaults.
- [[tmux_ai]] — LLM-augmented tmux wrapper that can drive your panes for you.

## Shells and prompts

- [[fish]] — friendly shell with autosuggestions and syntax highlighting built in (no plugins needed).
- See also [starship](https://starship.rs/) — the cross-shell prompt formerly stubbed here as `starshio.md`; now lives at `programming/rust/tooling/starship.md`.

## Editors

- [[helix]] — modal Rust editor with a Kakoune-style selection-then-action model and built-in LSP/tree-sitter.

## Git UIs

- [[lazygit]] — terminal UI that exposes git's full power without making you remember all the commands.

## Networking and remote sessions

- [[mosh]] — UDP-based "mobile shell" that survives roaming and flaky networks; predictive local echo over high-latency links.
- [[terminals]] — short comparison of remote-terminal stacks (Byobu / Mosh / Xpra).
- [[trace]] — Trippy, a combined traceroute + ping TUI for diagnosing routing issues.

## Debuggers and tracing

- [[debugger]] — Valgrind and friends; when to use which dynamic-analysis tool.

## Reading list

- [[books]] — curated long-form Linux reading (kernel internals, etc.).

## See also

- [[tools/shell/README|tools/shell]] — POSIX-shell utilities ([[lynx]], [[browsh]], [[sshpass]], [[dns_toys]], [[color_codes]]).
- [[tools/shell/must_have|must_have]] — "commands to install" cheat-sheet (killport, fd, fortune, powerlevel10k, etc.); reads naturally alongside this section.

## Keywords

`#linux` `#terminal` `#multiplexer` `#shell` `#editor` `#debugger` `#networking` `#tools`
