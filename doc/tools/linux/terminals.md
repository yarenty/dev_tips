---
title: "Terminal emulators and remote-terminal stacks"
main_link: https://github.com/mobile-shell/mosh
keywords: [terminals, byobu, mosh, xpra, remote, multiplexer, comparison, linux]
status: reviewed
review_date: 2026/05/03
---

# Terminal emulators and remote-terminal stacks

**Main link:** <https://github.com/mobile-shell/mosh>

Byobu: <https://www.byobu.org/> · Mosh: <https://mosh.org/> · Xpra: <https://github.com/Xpra-org/xpra>

## Summary

A short comparison of three open-source projects often confused for each other because they all do "something with a terminal over a network": **Byobu** (a multiplexer/status-bar wrapper over tmux/screen), **Mosh** (a UDP replacement for SSH that survives roaming), and **Xpra** (an "X11 over the network with disconnect/reconnect" — like `screen` for graphical X programs). All three are GPL-licensed.

## Insight

These solve overlapping but distinct problems:

| Tool      | What you keep          | Network         | Use it when                                                                  |
| --------- | ---------------------- | --------------- | ---------------------------------------------------------------------------- |
| **Mosh**  | One terminal session   | UDP, roaming    | You SSH into the same box from a flaky/changing network and want it to "just work". |
| **Byobu** | N windows + status bar | Whatever tmux uses (TCP via SSH) | You want persistent multi-window terminal sessions with sane defaults. |
| **Xpra**  | GUI X11 apps           | TCP/SSH/SSL     | You need to run a graphical Linux app remotely and disconnect/reconnect to it. |

The canonical "resilient remote dev box" recipe is **Mosh + Byobu (or tmux/zellij)**: Mosh handles the transport, the multiplexer handles persistence and panes. Add Xpra only if you need to drag GUI windows back from a remote machine.

For modern terminal *emulators* (Alacritty, Kitty, WezTerm, Ghostty), this article doesn't cover them yet — that survey lives separately. The picker shorthand: Alacritty for "fastest minimal", Kitty for "feature-rich with images", WezTerm for "Lua-configurable cross-platform", Ghostty for "fast and modern with batteries included".

## Similar / related topics

- [Byobu](https://www.byobu.org/) — see [[byobu]].
- [Mosh](https://mosh.org/) — see [[mosh]].
- [Xpra](https://github.com/Xpra-org/xpra) — "screen for X11" plus HTML5 client.
- [Eternal Terminal](https://eternalterminal.dev/) — TCP-based SSH alternative with reconnect.
- [[tools/linux/tmux|tmux]] / [[zellij]] — the multiplexers Mosh pairs with.

## Internal links

- [[byobu]]
- [[mosh]]
- [[tools/linux/tmux|tmux]]
- [[zellij]]

## Keywords

`#terminals` `#byobu` `#mosh` `#xpra` `#remote` `#multiplexer` `#comparison` `#linux`

## References / raw notes

### Project summary: Byobu, Mosh, Xpra

- **Source:** <https://github.com/dustinkirkland/byobu>
- **Source:** <https://github.com/mobile-shell/mosh>
- **Source:** <https://github.com/Xpra-org/xpra>

### TL;DR

- **Byobu:** A text-based window manager and terminal multiplexer enhancing GNU Screen / tmux functionality.
- **Mosh:** A remote terminal application that improves SSH by supporting roaming and intermittent connectivity.
- **Xpra:** A remote desktop / window manager that enables remote access to X11 programs with seamless session management (think "screen for X").
- All three are open source under GPL-family licenses (Byobu and Mosh are GPLv3).

### Recap

Three distinct open-source projects covering: terminal management (Byobu), resilient remote sessions (Mosh), and remote desktop / window management (Xpra).

### Open questions

1. Specific dependencies for installing Byobu on a minimal Debian image.
2. How Mosh manages prediction errors and latency on slow links.
3. Recommended Xpra installation steps per OS.
4. How Xpra handles different network protocols (SSH, SSL, raw TCP, WebSocket).
