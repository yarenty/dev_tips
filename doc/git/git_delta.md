---
title: delta — pretty diff & blame pager for Git
main_link: https://github.com/dandavison/delta
keywords: [git, delta, git-delta, diff, pager, syntax-highlighting, dandavison]
status: reviewed
review_date: 2026/05/03
---

# delta — pretty diff & blame pager for Git

**Main link:** <https://github.com/dandavison/delta>

## Summary

`delta` is a syntax-highlighting pager for `git diff`, `git log`, `git show`, `git stash show`, `git blame`, and `git reflog`. Written in Rust by Dan Davison, it replaces the bare red/green diff output with a syntax-aware, side-by-side or unified view, with file headers, line numbers, hunk highlighting, and configurable themes. It plugs in via a single `core.pager` setting; everything else is opt-in.

## Insight

Use it the moment you start reading more diffs than you write. The default `git diff` output is unreadable beyond about 30 lines; `delta` makes large reviews tractable. Pairs perfectly with the [[config|sane defaults]] snippet — in particular `diff.algorithm = histogram` (better hunk detection) and `merge.conflictStyle = zdiff3` (delta renders the third marker beautifully). The Rust install is one `brew install git-delta` (or `cargo install git-delta`) and there are no daemons / background processes — it's a one-shot pager so the cost is zero when you're not viewing diffs. Side-by-side mode (`delta.side-by-side = true`) is the killer feature for code review.

## Similar / related topics

- [[config]] — global Git defaults that complement delta.
- `diff-so-fancy` — older, perl-based predecessor; less feature-rich.
- `bat` — same syntax-highlighting library (`syntect`) used as a `cat` replacement.
- `difftastic` — structural / AST-aware diff (different goal: shows what *changed semantically*, not what changed line-wise).
- `tig` — TUI Git browser; can be configured to use delta.

## Internal links

- [[config]] — Global Git config that should pair with delta (`core.pager`, `interactive.diffFilter`, `delta.navigate`).
- [[github]] — Other Git tooling note in this section.

## Keywords

`#git` `#delta` `#git-delta` `#diff` `#pager` `#syntax-highlighting` `#dandavison`

## References / raw notes

- Project: <https://github.com/dandavison/delta>
- Install docs: <https://dandavison.github.io/delta/installation.html>

### Install (macOS)

```shell
brew install git-delta
```

Other platforms: `cargo install git-delta`, `apt install git-delta`, or grab a release binary from the GitHub releases page.

### Wire it into your global Git config

```shell
git config --global core.pager 'delta'
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true               # n/N to jump between files
git config --global merge.conflictStyle zdiff3        # delta renders zdiff3 nicely
```

Equivalent in `~/.gitconfig`:

```ini
[core]
    pager = delta
[interactive]
    diffFilter = delta --color-only
[delta]
    navigate = true
    light = false              # set true if your terminal background is light
    line-numbers = true
    side-by-side = true        # uncomment for side-by-side diffs
[merge]
    conflictStyle = zdiff3
```

### Useful one-offs

```shell
git diff | delta                       # ad-hoc diff
git log -p | delta                     # full history with diffs
git blame <file> | delta                # syntax-highlighted blame
git reflog --color=always | delta      # easier-to-read reflog
```
