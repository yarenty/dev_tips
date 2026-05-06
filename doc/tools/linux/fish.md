---
title: "Fish — the friendly interactive shell"
main_link: https://fishshell.com/
keywords: [fish, shell, friendly, autosuggestions, syntax-highlighting, no-config, posix]
status: reviewed
---

# Fish — the friendly interactive shell

**Main link:** <https://fishshell.com/>

Repo: <https://github.com/fish-shell/fish-shell>

## Summary

Fish (the friendly interactive shell) is a Unix shell that ships with autosuggestions, full-line syntax highlighting, sane tab completion, and a web-based config UI out of the box — no plugin manager, no 200-line `.zshrc`. It's not POSIX-compatible: `fish` is its own scripting language, deliberately simpler than `sh`/`bash`. Originally by Axel Liljencrantz, now maintained by a large community; rewritten from C++ to Rust starting with 4.0 (2025).

## Insight

The pitch is that everything you'd install plugins for in zsh — autosuggestions (`zsh-autosuggestions`), syntax highlighting (`zsh-syntax-highlighting`), better completions — is on by default and just works. A typical `~/.config/fish/config.fish` is under 30 lines, mostly abbreviations.

The honest trade-off: **Fish is not POSIX**. `&&`, `||`, `$()`, `[[ ]]`, `export FOO=bar` all change form (`and`, `or`, `()`, `test`, `set -x FOO bar`). Snippets you copy off the internet often need translating. Solutions:

- Keep `bash` as your script interpreter (`#!/usr/bin/env bash`) and use `fish` only as your interactive shell.
- For one-offs, just `bash -c "..."` from inside fish.

Pair fish with [starship](https://starship.rs/) (no config), [`z`](https://github.com/jethrokuan/z) for directory jumping, and you have a near-zero-config interactive setup. If you need POSIX, look at [`zsh`](https://www.zsh.org/) with [zinit](https://github.com/zdharma-continuum/zinit) or [`nushell`](https://www.nushell.sh/) (also non-POSIX, but structured-data first).

## Similar / related topics

- [zsh](https://www.zsh.org/) — POSIX-compatible, the macOS default; needs plugins to feel like fish.
- [bash](https://www.gnu.org/software/bash/) — the lowest common denominator on Linux servers.
- [nushell](https://www.nushell.sh/) — pipes carry structured data instead of bytes.
- [starship](https://starship.rs/) — cross-shell prompt; the natural pair with fish.
- [[helix]] — modal Rust editor; another "no-config" tool in the same spirit.
- [[tools/linux/tmux|tmux]] — the multiplexer to drive your fish sessions.

## Internal links

<!-- reviewed -->

- [[helix]]
- [[tools/linux/tmux|tmux]]
- [[zellij]]
- [[lazygit]]

## Keywords

`#fish` `#shell` `#friendly` `#autosuggestions` `#syntax-highlighting` `#no-config` `#linux` `#cli`

## References / raw notes

### Why fish

Julia Evans posted [Reasons I still love the fish shell](https://jvns.ca/blog/2024/01/04/reasons-i-still-love-fish/), and the first point she makes is "no configuration".

Things that require plugins and lots of code in shells like zsh — autosuggestions, syntax highlighting — are included and configured by default in fish. A small `config.fish` of <31 lines is enough for most setups, mostly abbreviations.

### Recommended plugins

- **z** — jump to a directory by frecency.
- **hydro** — minimal async shell prompt (or use [starship](https://starship.rs/) instead).

Neither needs configuration.

### Installing

```shell
# macOS
brew install fish

# Debian/Ubuntu
sudo apt install fish

# Arch
sudo pacman -S fish

# Make it your login shell (after adding it to /etc/shells)
chsh -s (which fish)
```

### Starship prompt

The cross-shell prompt that pairs naturally with fish lives at <https://starship.rs/> (and `programming/rust/tooling/starship.md` in this vault).

```shell
curl -sS https://starship.rs/install.sh | sh
# then add to ~/.config/fish/config.fish:
# starship init fish | source
```
