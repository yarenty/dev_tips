---
title: "Shell-script ergonomics toolkit (forgit, gum)"
main_link: https://github.com/charmbracelet/gum
keywords: [shell-tools, gum, forgit, charmbracelet, fzf, interactive, scripting, shell]
status: reviewed
review_date: 2026/05/03
---

# Shell-script ergonomics toolkit (forgit, gum)

**Main link:** <https://github.com/charmbracelet/gum>

A small kit of utilities that make interactive shell scripts and git work
feel less like 1985.

## Summary

This page collects the two most-loved tools in the "make your shell life
prettier and more interactive" category: `forgit` (an `fzf`-driven UI on
top of git) and `gum` (a Charm-cli toolkit for prompting, choosing,
spinning, and styling output from any shell script). Either alone is a
quality-of-life upgrade; together they cover most "I wish bash had a UI"
moments.

## Insight

`forgit` is the easier sell — install it, type `ga` instead of
`git add -p`, and you'll never go back. The fuzzy preview turns surgical
staging from a chore into something fun.

`gum` is the right answer when you find yourself reaching for `read -p`,
ANSI [[color_codes]] strings, or copy-pasted "spinner" loops in a shell
script. It produces real TUIs from one-line invocations and pipes
seamlessly with the rest of the shell. The trade-off: it's an external
binary, so anything you ship has to bundle or document the dependency.

For complex multi-screen TUIs in Go, graduate from `gum` to its parent
[Bubble Tea](https://github.com/charmbracelet/bubbletea); for Rust, use
[`ratatui`](https://ratatui.rs/) or [[tokio]] + a TUI crate.

## Similar / related topics

- [`fzf`](https://github.com/junegunn/fzf) — the fuzzy finder both `forgit` and many other tools build on.
- [[lazygit]] — full-screen git TUI; complementary to `forgit`'s inline approach.
- [Bubble Tea](https://github.com/charmbracelet/bubbletea) — Charm's full TUI framework when `gum` outgrows you.
- [`fx`](https://github.com/antonmedv/fx) — interactive JSON viewer in the same spirit.
- [[must_have]] — sister page of "install on every fresh box" CLI tools.

## Internal links

- [[must_have]]
- [[lazygit]]
- [[color_codes]]
- [[fish]]

## Keywords

`#shell-tools` `#gum` `#forgit` `#charmbracelet` `#fzf` `#interactive` `#scripting` `#shell`

## References / raw notes

### forgit — fzf-driven git UI

<https://github.com/wfxr/forgit>

Utility tool for using git interactively. Powered by [`junegunn/fzf`](https://github.com/junegunn/fzf).

Install per shell:

```shell
# fish via fisher
fisher install wfxr/forgit

# fish via oh-my-fish
omf install https://github.com/wfxr/forgit

# zsh via oh-my-zsh
git clone https://github.com/wfxr/forgit.git \
  "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/forgit"
# then add `forgit` to plugins=( ... ) in ~/.zshrc

# bash
git clone https://github.com/wfxr/forgit ~/.forgit
echo 'source ~/.forgit/forgit.plugin.sh' >> ~/.bashrc
```

Aliases you'll use daily once installed:

| alias | does                                |
|-------|-------------------------------------|
| `ga`  | `git add` with diff preview         |
| `glo` | log with diff preview               |
| `gd`  | diff browser                        |
| `gcb` | checkout branch                     |
| `gcf` | checkout file                       |
| `grh` | reset HEAD                          |
| `gss` | stash viewer                        |

### gum — glamorous shell scripts

<https://github.com/charmbracelet/gum>

A toolkit for shell scripts powered by Charm's [Bubbles](https://github.com/charmbracelet/bubbles)
and [Lip Gloss](https://github.com/charmbracelet/lipgloss). No Go code
required — every primitive is a one-liner CLI.

Install:

```shell
brew install gum
# or
go install github.com/charmbracelet/gum@latest
```

Building blocks (each prints to stdout, exits non-zero on cancel):

```shell
# pick one option
TYPE=$(gum choose "fix" "feat" "docs" "style" "refactor" "test" "chore" "revert")

# single-line input
SCOPE=$(gum input --placeholder "scope")

# multi-line input
DESCRIPTION=$(gum write --placeholder "Details of this change")

# yes/no confirmation
gum confirm "Commit changes?" && echo "yes"
```

Putting it all together — a Conventional-Commits commit helper:

```shell
#!/bin/sh
TYPE=$(gum choose "fix" "feat" "docs" "style" "refactor" "test" "chore" "revert")
SCOPE=$(gum input --placeholder "scope")

# Wrap scope in parens if non-empty
[ -n "$SCOPE" ] && SCOPE="($SCOPE)"

SUMMARY=$(gum input --value "$TYPE$SCOPE: " --placeholder "Summary of this change")
DESCRIPTION=$(gum write --placeholder "Details of this change")

gum confirm "Commit changes?" && git commit -m "$SUMMARY" -m "$DESCRIPTION"
```

Other useful primitives: `gum spin -- long-running-cmd`, `gum style`,
`gum filter`, `gum format`, `gum table`. See the [examples directory](https://github.com/charmbracelet/gum/tree/main/examples).
