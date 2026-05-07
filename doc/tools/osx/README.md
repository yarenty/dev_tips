---
title: "macOS / Apple platform tooling"
main_link: https://developer.apple.com/
keywords: [macos, osx, apple, ios, xcode, virtualization, cross-compile, apple-silicon, utm, osxcross]
status: reviewed
review_date: 2026/05/03
---

# macOS / Apple platform tooling

**Main link:** <https://developer.apple.com/>

This section collects notes on tooling that is **specific to macOS / Apple hardware**: iOS development, macOS-only desktop quirks, virtualization on Apple Silicon, and cross-compiling *to* macOS from other hosts.

Cross-platform shell / terminal tooling that happens to run on a Mac (fish, helix, tmux, lazygit, mosh, etc.) lives under [`tools/linux/`](../linux/README.md) instead, since most of it originated on Linux and runs identically on macOS via Homebrew.

## Articles

- [ios](ios.md) — Apple's iOS developer portal entry points: review, debugging, and App Store guidelines.
- [osx_tricks](osx_tricks.md) — Tip jar for macOS Gatekeeper / quarantine / `spctl` workarounds when third-party apps refuse to open.
- [osxcross](osxcross.md) — Cross-toolchain (clang + cctools + macOS SDK) for building macOS binaries from Linux, FreeBSD, or Android hosts.
- [utm](utm.md) — Free QEMU-based virtualizer for macOS, with first-class Apple Silicon support via Apple's Hypervisor framework.

## Insight

The four articles here cluster into three distinct workflows:

1. **Building *for* Apple platforms** — `ios.md` (App Store / Xcode side) and `osxcross.md` (CI side: producing Mach-O binaries without owning a Mac, or as part of a multi-platform release pipeline).
2. **Running other OSes on a Mac** — `utm.md` for general VMs. For Linux specifically, [Lima](https://github.com/lima-vm/lima) and [Colima](https://github.com/abiosoft/colima) are usually more ergonomic than UTM (CLI-driven, headless, automatic file sharing, Docker integration).
3. **Surviving macOS itself** — `osx_tricks.md` for Gatekeeper friction and similar "the OS won't let me run my own software" papercuts.

What's deliberately *not* here: anything Homebrew-installable that's identical on Linux. If you're looking for a terminal multiplexer, shell, editor, or git TUI, check `tools/linux/` and `tools/shell/`.

## Similar / related topics

- [[tools/linux/books|books]] — general dev-tooling reading list, much of which applies on macOS.
- [[tools/linux/terminals|terminals]] — terminal emulators (Alacritty, Kitty, WezTerm, Ghostty) all run on macOS.
- [[tools/linux/tmux|tmux]] / [[zellij]] / [[byobu]] — multiplexers that work the same on macOS.
- [Lima](https://github.com/lima-vm/lima) / [Colima](https://github.com/abiosoft/colima) — lighter Linux-on-Mac alternatives to UTM for dev work.
- [Asahi Linux](https://asahilinux.org/) — running native Linux on Apple Silicon, the inverse of UTM.

## Internal links

- [[tools/linux/books|books]]
- [[tools/linux/terminals|terminals]]
- [[tools/linux/tmux|tmux]]
- [[fish]]
- [[helix]]

## Keywords

`#macos` `#osx` `#apple` `#ios` `#xcode` `#apple-silicon` `#virtualization` `#cross-compile` `#utm` `#osxcross`
