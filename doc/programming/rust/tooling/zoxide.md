---
title: zoxide — smarter `cd` (autojump replacement)
main_link: https://github.com/ajeetdsouza/zoxide
keywords: [zoxide, rust, cd, autojump, z, fasd, shell, navigation]
status: reviewed
---

# zoxide — smarter `cd` (autojump replacement)

**Main link:** <https://github.com/ajeetdsouza/zoxide>

## Summary

`zoxide` is a smarter `cd` command by Ajeet D'Souza, inspired by `z` and `autojump`. It tracks directories you `cd` into and ranks them by [frecency](https://en.wikipedia.org/wiki/Frecency) (frequency × recency), so `z proj` jumps straight to `~/work/projects/awesome-thing` after a few uses. It works on any shell (bash / zsh / fish / nushell / pwsh / xonsh) via a one-liner shell init, and the binary is fast (Rust + sqlite-free flat-file db at `~/.local/share/zoxide/`).

## Insight

Reach for `zoxide` if you spend half your terminal life typing `cd ~/work/clients/foo/bar/cmd/some-svc/internal`. After the first manual `cd` the path is in the database; thereafter `z svc` is enough. The killer addition is `zi` — interactive fuzzy pick across the database (uses `fzf` if installed). Common patterns:

```sh
z foo                  # jump by partial name match (frecency-ranked)
z foo bar              # match dirs containing both substrings
z -                    # like cd -
zi                     # interactive picker (needs fzf)
zoxide query foo       # show what z would jump to
zoxide remove ~/old    # forget a path
```

**Setup gotcha**: the `init` line goes in your shell rc *after* anything else that wraps `cd` (e.g. nvm). It re-aliases `cd` to `z` by default — use `--cmd cd` if you want, or pick a different alias if you don't want to override `cd`. **Compared to siblings**: **`autojump`** (Python) and **`z.sh`** (bash) are the originals; both still work but are slower and platform-fussier; **`fasd`** is dead (last commit 2017) but covered the file-jump use case too. Pair with `[[../../../tools/shell/must_have|must_have]]` and `[[starship]]` for a complete fresh-box shell setup.

## Similar / related topics

- `cd`(1) — the POSIX original.
- [`autojump`](https://github.com/wting/autojump) — Python predecessor; similar ranking idea.
- [`z.sh`](https://github.com/rupa/z) — original bash implementation; lighter but slower.
- [`fasd`](https://github.com/clvv/fasd) — file-and-dir jumper; unmaintained.
- [`fzf`](https://github.com/junegunn/fzf) — pairs with `zi` for fuzzy interactive jumping.

## Internal links

- [[README]] — tooling section landing.
- [[starship]] — pairs naturally as part of a shell-productivity stack.
- [[../../../tools/shell/must_have|must_have]] — fresh-box CLI bundle that includes zoxide.

## Keywords

`#zoxide` `#rust` `#cd` `#shell` `#navigation` `#autojump` `#frecency` `#cli`

## References / raw notes

- Repo: <https://github.com/ajeetdsouza/zoxide>
- Crate: <https://crates.io/crates/zoxide>
- Install: `cargo install zoxide`, `brew install zoxide`, or `apt install zoxide`.
- Shell init (bash example): `eval "$(zoxide init bash)"`.
