---
title: "Byobu — text-based window manager and terminal multiplexer"
main_link: https://www.byobu.org/
keywords: [byobu, tmux, screen, multiplexer, ubuntu, terminal, status-bar, gplv3]
status: reviewed
review_date: 2026/05/03
---

# Byobu — text-based window manager and terminal multiplexer

**Main link:** <https://www.byobu.org/>

Repo: <https://github.com/dustinkirkland/byobu>

## Summary

Byobu is a configuration layer and curses-based status wrapper that sits on top of GNU Screen or tmux. It was created by Dustin Kirkland (then at Canonical) to give Ubuntu server users a friendlier multiplexer out of the box: F-key shortcuts, persistent named sessions, and a status bar showing load average, RAM, disk, IP, uptime, etc. It's licensed GPLv3 and ships in most Linux distributions.

## Insight

Reach for Byobu when you want tmux's persistence and split panes without learning the prefix-key dance, or when you're SSH'd into a server you don't own and want a sane status line in seconds (`sudo apt install byobu && byobu`). The F-key bindings (`F2` new window, `F3`/`F4` switch, `F6` detach) are easier to teach to teammates than `Ctrl-b ?`.

The trade-off: it's still tmux/screen underneath, so you inherit their quirks; configuration ends up scattered across `~/.byobu/` *and* `~/.tmux.conf`. If you're already comfortable with tmux, Byobu mostly just adds a status bar — at that point [[zellij]] is the more interesting modern alternative, while purists will stay on bare [[tools/linux/tmux|tmux]].

## Similar / related topics

- [[tools/linux/tmux|tmux]] — what Byobu is wrapping by default since ~2011.
- [[zellij]] — modern Rust multiplexer with a discoverable status bar and floating panes built in.
- [[mosh]] — pairs nicely with Byobu for resilient remote sessions over flaky links.
- [GNU Screen](https://www.gnu.org/software/screen/) — Byobu's original backend; still selectable via `byobu-select-backend`.
- [[terminals]] — comparison of Byobu / Mosh / Xpra for remote work.

## Internal links

- [[tools/linux/tmux|tmux]]
- [[zellij]]
- [[mosh]]
- [[terminals]]

## Keywords

`#byobu` `#tmux` `#screen` `#multiplexer` `#ubuntu` `#terminal` `#status-bar` `#linux`

## References / raw notes

### What it is

- Open-source text-based window manager and terminal multiplexer.
- Enhances GNU Screen / tmux with status notifications, F-key bindings, and named sessions.
- Runs on most Linux, BSD, and macOS distributions.
- License: GPLv3. Originally maintained by Dustin Kirkland.

### Quick start

```shell
sudo apt install byobu       # Debian/Ubuntu
brew install byobu           # macOS
byobu                        # start a session
byobu-select-backend         # pick tmux or screen as the underlying engine
byobu-enable                 # auto-launch byobu on every login shell
```

### Default key bindings

| Key  | Action                          |
| ---- | ------------------------------- |
| `F2` | Create a new window             |
| `F3` | Previous window                 |
| `F4` | Next window                     |
| `F6` | Detach from session             |
| `F7` | Enter scrollback / copy mode    |
| `F8` | Rename current window           |
| `F9` | Configuration menu              |

### Source

- <https://github.com/dustinkirkland/byobu>
- <https://www.byobu.org/>
