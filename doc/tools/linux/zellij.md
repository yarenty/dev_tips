---
title: "Zellij — modern Rust terminal multiplexer"
main_link: https://zellij.dev/
keywords: [zellij, multiplexer, terminal, rust, panes, layouts, floating, plugins]
status: reviewed
review_date: 2026/05/03
---

# Zellij — modern Rust terminal multiplexer

**Main link:** <https://zellij.dev/>

Repo: <https://github.com/zellij-org/zellij>

## Summary

Zellij is a terminal multiplexer written in Rust, built around the goal of making tmux/screen functionality *discoverable*. It opens with a status bar showing the current keybindings, supports floating panes natively, and is configured through a single KDL file (or no file at all — the defaults are sane). The project is community-led, with first-class support for WebAssembly plugins.

## Insight

The pitch is "tmux but you can figure it out without reading the manual". Concretely:

- **Status bar at the bottom** shows the current mode and the next keys you can press. No `Ctrl-b ?` cheat-sheet round-trip.
- **Floating panes** are first-class (`Ctrl-p w`). Drop Lazygit / a quick shell on top of your editor without splitting.
- **Layouts** are KDL files — version-controllable, no scripting required for "open my dev environment".
- **Locked mode** (`Ctrl-g`) lets keys pass through to apps that fight a multiplexer (Vim, Helix, etc.).
- **WASM plugin system** — plugins are `.wasm` files, sandboxed, language-agnostic.

When *not* to pick Zellij over [[tools/linux/tmux|tmux]]:

- You SSH into many random servers — tmux is preinstalled almost everywhere; Zellij isn't (yet).
- You depend on a specific tmux plugin (resurrect, continuum) that has no Zellij equivalent.
- You script your multiplexer heavily — tmux's `tmux send-keys` / `tmux split-window` interface is much more mature.

For a fresh local setup, Zellij is the easier sell. Pair with [[helix]] and [[lazygit]] in floating panes for a near-zero-config TUI dev environment.

## Similar / related topics

- [[tools/linux/tmux|tmux]] — the incumbent; far bigger plugin ecosystem and ubiquity on remote boxes.
- [[byobu]] — opinionated wrapper around tmux/screen with a similar "discoverability" goal.
- [GNU Screen](https://www.gnu.org/software/screen/) — the original multiplexer; mostly historical interest.
- [WezTerm multiplexer](https://wezfurlong.org/wezterm/multiplexing.html) — built into the WezTerm emulator if you live there.
- [[helix]] / [[lazygit]] — natural floating-pane companions.

## Internal links

- [[tools/linux/tmux|tmux]]
- [[byobu]]
- [[helix]]
- [[lazygit]]

## Keywords

`#zellij` `#multiplexer` `#terminal` `#rust` `#panes` `#layouts` `#floating` `#wasm` `#linux`

## References / raw notes

### What it is

A batteries-included tmux alternative. Zellij doesn't need any configuration to work well. You can set up Layouts without additional plugins (although there is a plugin system) and most tmux configuration needs disappear.

Favourite feature: **floating panes**. Press `Ctrl-p`, then `w` to toggle a pane floating on top of everything else. Excellent for Lazygit on top of an editor.

### Install

```shell
# Cargo
cargo install --locked zellij

# Homebrew
brew install zellij

# Arch
sudo pacman -S zellij
```

### Default keybindings (high level)

| Keys        | Action                          |
| ----------- | ------------------------------- |
| `Ctrl-p`    | Pane mode (then `n` new, `x` close, `w` toggle floating, arrows to move) |
| `Ctrl-t`    | Tab mode                        |
| `Ctrl-s`    | Scroll / search mode            |
| `Ctrl-o`    | Session mode (detach with `d`)  |
| `Ctrl-g`    | Lock mode — pass keys to inner app |
| `Ctrl-h`    | Move focus left                 |
| `Ctrl-q`    | Quit                            |

### Minimal config (`~/.config/zellij/config.kdl`)

```kdl
theme "kanagawa"

default_shell "fish"

// Optional: hide the status bar to mimic a stock tmux look
// default_layout "compact"
```

### Layouts

Save a workspace as a KDL file under `~/.config/zellij/layouts/dev.kdl` and launch with `zellij --layout dev`. Layouts can pre-spawn editor + git pane + log pane in one command — roughly equivalent to a tmuxinator/teamocil config but built in.
