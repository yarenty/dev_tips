---
title: "Lazygit — terminal UI for git"
main_link: https://github.com/jesseduffield/lazygit
keywords: [lazygit, git, tui, magit, gitui, tig, jesseduffield, version-control]
status: reviewed
---

# Lazygit — terminal UI for git

**Main link:** <https://github.com/jesseduffield/lazygit>

Docs: <https://github.com/jesseduffield/lazygit#tutorials>

## Summary

Lazygit is a fast, keyboard-driven terminal UI for git, written in Go by Jesse Duffield. It exposes most of git's day-to-day operations — staging hunks, interactive rebase, cherry-pick, stash, branch management, conflict resolution — through panes you navigate with single-letter keys. No plugin or shell integration required; one binary, runs on Linux/macOS/Windows.

## Insight

The pitch: **git's full power without remembering all the flags**. Hunk-level staging is a single keypress (`space`); interactive rebase is a visual list with reorder/squash/edit on letter keys; resolving merge conflicts opens a side-by-side diff with `1` / `2` / `b` to pick. The "I'll just `git reset --hard` and start over" instinct dies after a week of Lazygit because every operation is reversible and visible.

Picker:

- **You live in a terminal and use plain git**: Lazygit. Best ergonomics-per-minute-learned in the space.
- **You live in Emacs**: [Magit](https://magit.vc/) — still the gold standard, but only inside Emacs.
- **You want lighter / read-mostly**: [tig](https://jonas.github.io/tig/) — a long-running, ncurses-flavoured browser for log/diff/blame.
- **You want a Rust-y similar TUI**: [gitui](https://github.com/extrawurst/gitui) — faster on huge repos but with a smaller feature surface than Lazygit.

It pairs especially well with [[zellij]]'s floating panes (or tmux popup windows) — bind a key to toggle Lazygit on top of your editor, and `git` essentially disappears from your CLI muscle memory.

## Similar / related topics

- [Magit](https://magit.vc/) — the Emacs git porcelain that started this whole genre.
- [tig](https://jonas.github.io/tig/) — older, browser-flavoured TUI.
- [gitui](https://github.com/extrawurst/gitui) — Rust alternative; very fast on big repos.
- [GitHub CLI `gh`](https://cli.github.com/) — for PRs, issues, gists; complementary, not overlapping.
- [[zellij]] — toggle Lazygit in a floating pane.
- [[helix]] — pair editor + Lazygit for a tight TUI dev loop.

## Internal links

<!-- reviewed -->

- [[helix]]
- [[zellij]]
- [[tools/linux/tmux|tmux]]
- [[fish]]

## Keywords

`#lazygit` `#git` `#tui` `#magit` `#gitui` `#tig` `#version-control` `#linux`

## References / raw notes

### Why

After raving about Magit (in London), my team showed me Lazygit and I've been using it ever since — it's really good and it does exactly what you want it to do, without configuration.

You can toggle different screen modes, panes adjust in size when active, and pretty much everything you want to do is only a few keystrokes away.

### Install

```shell
# macOS
brew install lazygit

# Debian/Ubuntu (PPA)
sudo add-apt-repository ppa:lazygit-team/release
sudo apt update && sudo apt install lazygit

# Arch
sudo pacman -S lazygit

# Via Go
go install github.com/jesseduffield/lazygit@latest
```

### Cheat-sheet (most useful)

| Key       | Action                                  |
| --------- | --------------------------------------- |
| `space`   | Stage / unstage file or hunk            |
| `c`       | Commit                                  |
| `A`       | Amend last commit                       |
| `P` / `p` | Push / pull                             |
| `s`       | Stash                                   |
| `r`       | Rebase / interactive rebase from commit |
| `M`       | Mark commit / move within rebase        |
| `?`       | Show keybindings for current panel      |
