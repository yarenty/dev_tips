---
title: starship — cross-shell, language-aware prompt
main_link: https://starship.rs/
keywords: [starship, rust, prompt, shell, zsh, bash, fish, powerline]
status: reviewed
---

# starship — cross-shell, language-aware prompt

**Main link:** <https://starship.rs/>

## Summary

`starship` is a fast, minimal, infinitely customisable prompt for any shell, written in Rust. It works on bash, zsh, fish, PowerShell, ion, elvish, tcsh, nushell, xonsh, and cmd; activated with a single `eval "$(starship init <shell>)"` line. The prompt auto-detects context (git repo + branch + status, language toolchain version for ~30 languages, container/k8s context, AWS profile, battery, command duration, exit status) and shows only what's relevant.

## Insight

Reach for `starship` if you want a prompt that *just works* — sane defaults, no shell-specific config, configured by a single `~/.config/starship.toml`. The killer feature is the **toml config that's the same on every shell and machine**: dotfile sync once, prompt ready everywhere. Compared to:

- **`oh-my-zsh` themes / `agnoster` / `powerline-shell`** (Python) — older, slower, zsh-only or shell-specific.
- **`pure` / `lean` / `spaceship`** (zsh) — fast and minimal but zsh-only.
- **`p10k` (powerlevel10k)** — zsh-only; arguably faster than starship at the extreme but with much heavier config; instant-prompt mode is the killer feature p10k still does better.
- **Plain `PS1=…`** — fastest, infinite work to get the same features.

Pick `starship` if you switch shells / have multiple OSes; pick `p10k` if you live in zsh and want maximal speed.

**Gotchas**: the prompt *does* spawn the language-version detector for every prompt redraw (e.g. `node --version` in a node repo). On slow boxes or NFS this adds up; the `~/.config/starship.toml` lets you `disabled = true` on modules you don't need (`[python] disabled = true`). Nerd Font is required for the icon glyphs to render; install one of [nerdfonts.com](https://www.nerdfonts.com/) and set your terminal to use it.

Pair with [[zoxide]] (smarter cd) for a complete shell-productivity setup; both are typically on `must_have` lists for a fresh box.

## Similar / related topics

- `oh-my-zsh` themes — older, zsh-only, slower.
- `pure` / `lean` (Sindre Sorhus) — minimal zsh prompts.
- `powerlevel10k` — zsh-only; very fast with instant-prompt.
- `powerline-shell` — Python; slower; older niche.
- [[zoxide]] / [[bat]] — natural pairings in a CLI productivity stack.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[zoxide]] — pairs naturally as part of a shell-productivity setup.
- [[../../../tools/shell/must_have|must_have]] — fresh-box bundle.

## Keywords

`#starship` `#rust` `#prompt` `#shell` `#zsh` `#bash` `#fish` `#cross-shell`

## References / raw notes

- Site: <https://starship.rs/>
- Repo: <https://github.com/starship/starship>
- Install: `curl -sS https://starship.rs/install.sh | sh` or `brew install starship` or `cargo install starship --locked`.
- Activate: `eval "$(starship init bash)"` (or zsh / fish / etc.) in your shell rc.
- Config: `~/.config/starship.toml`.
