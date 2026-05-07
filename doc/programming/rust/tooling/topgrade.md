---
title: topgrade — orchestrate updates across all your package managers
main_link: https://github.com/topgrade-rs/topgrade
keywords: [topgrade, rust, package-manager, update, upgrade, brew, apt, cargo]
status: reviewed
---

# topgrade — orchestrate updates across all your package managers

**Main link:** <https://github.com/topgrade-rs/topgrade>

## Summary

`topgrade` is a Rust CLI that detects the package managers and update tools installed on your machine and runs each one in sequence — `brew`, `apt`, `dnf`, `pacman`, `cargo install-update`, `rustup`, `nvm`/`npm`, `pip`, `gem`, `go`, `flutter`, `tldr`, `vim` plugins, `tmux` plugins, neovim, OS updates (Windows Update, macOS softwareupdate), Docker images, and dozens more. The original repo by Roey Darwish was archived in 2022; the active community fork at [`topgrade-rs/topgrade`](https://github.com/topgrade-rs/topgrade) is what to install today.

## Insight

Reach for `topgrade` on a fresh laptop / dev VM where you've accumulated five or six package managers and "update everything" is otherwise a 10-line shell script you forgot half of. It is **not** a package manager itself — it just runs the right updaters in the right order, surfaces failures, and gives you one exit code. Common patterns:

```sh
topgrade                  # do everything detected
topgrade --dry-run        # show what would run
topgrade --disable cargo  # skip a step this time
topgrade --only brew npm  # only specific steps
```

Configuration lives at `~/.config/topgrade.toml` and is the right place to permanently disable a step you don't trust (e.g. `system` updates on a managed work laptop) or add custom pre/post commands.

**Gotchas**: it is *intentionally noisy* — every step gets a banner. Some steps prompt for sudo; on CI/headless boxes pass `--no-sudo` or pre-warm the sudo timestamp. Don't run it before a critical demo: package upgrades can land breaking changes (e.g. `brew upgrade git` once changed `git push` defaults). For the "I want to know what changed" follow-up, pair with `[[../../../tools/shell/must_have|must_have]]` and a system-snapshot tool.

Compared to siblings: there's no real direct competitor — every alternative is "write your own shell loop". The Nix/NixOS world handles this declaratively (`nixos-rebuild`/`home-manager switch`) but that's a different model.

## Similar / related topics

- Hand-rolled shell scripts that loop over `brew upgrade && apt upgrade && cargo install-update -a && rustup update && ...`.
- `[m]ackup` / `chezmoi` — dotfile sync, complementary to topgrade.
- Nix / `nixos-rebuild switch` — declarative alternative; whole different paradigm.
- `apt-get autoremove` / `brew cleanup` — pair after topgrade for housekeeping.

## Internal links

- [[README]] — tooling section landing.
- [[../../../tools/shell/must_have|must_have]] — fresh-box CLI bundle.
- [[starship]] / [[zoxide]] / [[bat]] — siblings in a healthy CLI stack that topgrade keeps current.

## Keywords

`#topgrade` `#rust` `#package-manager` `#update` `#upgrade` `#cli` `#brew` `#apt`

## References / raw notes

- Active fork: <https://github.com/topgrade-rs/topgrade>
- Original (archived): <https://github.com/r-darwish/topgrade>
- Crate: <https://crates.io/crates/topgrade>
- Install: `cargo install topgrade` or `brew install topgrade`.
