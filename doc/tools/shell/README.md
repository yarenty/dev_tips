---
title: "POSIX shell utilities"
keywords: [shell, posix, terminal, browsers, ssh, dns, scripting, ansi]
status: reviewed
review_date: 2026/05/03
---

# POSIX shell utilities

This section collects POSIX-shell-side utilities — things you reach for
from `bash`, `zsh`, or [[fish]] regardless of OS. Linux-specific tooling
lives under [`tools/linux/`](../linux/README.md) (including the canonical
[[tools/linux/tmux|tmux]] article), and macOS-specific tooling under
[`tools/osx/`](../osx/README.md).

## Articles

### Text-mode browsers

How to look at the web from a terminal — three options on a spectrum from
"renders the actual web platform" to "instant ASCII".

- [[carbonyl]] — Chromium-in-terminal: real JS, video, mouse, the lot.
- [[browsh]] — Firefox-backed text browser; the SSH-friendly middle ground.
- [[lynx]] — the classic text-only browser; perfect for "does this load without JS?" smoke tests.

### Shell scripts & ergonomics

Tools that make interactive shell life more pleasant and shell scripts
themselves more powerful.

- [[must_have]] — opinionated list of CLI tools (killport, fd, cmatrix, fortune, oh-my-zsh, powerlevel10k) to install on every fresh box.
- [[tools/shell/tools|shell tools]] — `forgit` (fzf-driven git UI) and `gum` (TUI primitives for shell scripts).
- [[color_codes]] — ANSI / 256 / truecolour escape-code cheat sheet, with Bash variables and a Rust example.

### SSH helpers

- [[sshpass]] — non-interactive password auth for SSH/SCP, with the security caveats that implies.

### DNS tricks

- [[dns_toys]] — a public DNS server that answers `dig` queries with weather, currency, time, CIDR maths, π…

## See also

- [[tools/linux/tmux|tmux]] — terminal multiplexer; lives in the Linux section because the canonical article does.
- [[fish]] — friendlier interactive shell; the only one with built-in autosuggestions out of the box.
- [[helix]] — modern modal editor; pairs with everything on this page.
- [[mosh]] — the latency-tolerant SSH replacement; pairs especially well with [[browsh]] and [[sshpass]] alternatives.
- [[byobu]], [[zellij]] — alternatives to tmux when you want a friendlier or more modern multiplexer.

## Keywords

`#shell` `#posix` `#terminal` `#browsers` `#ssh` `#dns` `#scripting` `#ansi`
