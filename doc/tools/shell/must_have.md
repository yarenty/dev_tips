---
title: "Must-have CLI tools for a fresh box"
main_link: https://github.com/jkfran/killport
keywords: [must-have, cli, killport, fd, cmatrix, fortune, oh-my-zsh, powerlevel10k, shell]
status: reviewed
---

# Must-have CLI tools for a fresh box

**Main link:** <https://github.com/jkfran/killport>

A short, opinionated list of utilities I install on every fresh
macOS / Linux box before doing real work. None of these are essential — but
each one removes a recurring papercut.

## Summary

This is a "first hour on a new machine" recipe: kill stuck processes by
port, find files without remembering find's flags, and make the prompt
itself stop being ugly. Everything here installs in one line via Homebrew
or `cargo`. None of it requires sudo on macOS once Homebrew is set up.

## Insight

Two opinions worth stating up front:

- **`fd`, `rg`, `bat`, `eza`** are strictly better than `find`, `grep`,
  `cat`, `ls` for interactive use. Add them and stop fighting POSIX flags.
  (Don't alias them over the originals — scripts depend on the old ones.)
- **A nice prompt earns its keep**, but [`starship`](https://starship.rs/)
  is now a much easier choice than `powerlevel10k` because it's shell-
  agnostic (works with [[fish]] too) and zero-config.

The toy entries (`cmatrix`, `sl`, `fortune`) are kept because they make
demoing a new shell setup more fun and they're trivial to skip.

## Similar / related topics

- [[tools/shell/tools|shell tools]] — sister page for shell-script ergonomics (`gum`, `forgit`).
- [[fish]] — friendlier interactive shell than bash/zsh; pairs with starship.
- [[helix]] — modern modal editor, install it the same day.
- [`starship`](https://starship.rs/) — the prompt I use instead of powerlevel10k these days.
- [[lazygit]] — TUI git client; fits naturally on this list.

## Internal links

<!-- reviewed -->

- [[tools/shell/tools|shell tools]]
- [[fish]]
- [[helix]]
- [[lazygit]]
- [[color_codes]]

## Keywords

`#must-have` `#cli` `#killport` `#fd` `#oh-my-zsh` `#powerlevel10k` `#shell` `#starship`

## References / raw notes

### killport — kill the process holding a port

<https://github.com/jkfran/killport>

```shell
cargo install killport
# or
brew install killport

killport 3000
```

### fd — a friendlier `find`

<https://github.com/sharkdp/fd>

```shell
brew install fd
# or
cargo install fd-find

fd '\.rs$'        # find Rust files in cwd
fd -e md          # by extension
fd -H .gitignore  # include hidden
```

### cmatrix — Matrix-style screensaver

```shell
brew install cmatrix

cmatrix -s -C red
```

### sl — joke train when you mistype `ls`

```shell
brew install sl
```

### fortune — random quotes (good for MOTD)

```shell
brew install fortune
fortune
```

### oh-my-zsh / oh-my-fish

The community frameworks for zsh and fish. Pick one based on your shell:

- zsh: <https://ohmyz.sh/>
- fish: <https://github.com/oh-my-fish/oh-my-fish>

### powerlevel10k — fast, configurable zsh prompt

<https://github.com/romkatv/powerlevel10k>

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
```

Then set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc` and run
`p10k configure`.

> Modern alternative: [`starship`](https://starship.rs/) — same look, works
> in any shell, single binary, zero config to start.
