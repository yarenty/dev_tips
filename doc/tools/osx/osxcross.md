---
title: "OSXCross — cross-compile to macOS from Linux"
main_link: https://github.com/tpoechtrager/osxcross
keywords: [osxcross, cross-compile, macos, clang, cctools, ld64, sdk, linux, ci]
status: reviewed
review_date: 2026/05/03
---

# OSXCross — cross-compile to macOS from Linux

**Main link:** <https://github.com/tpoechtrager/osxcross>

## Summary

OSXCross (by Thomas Pöchtrager) is a collection of scripts that wires up a working **macOS cross-toolchain on a non-Mac host** — Linux, FreeBSD, OpenBSD, or Android (Termux). It pairs the system's Clang/LLVM with a port of Apple's `cctools` (`lipo`, `otool`, `nm`, `ar`, `ld64`) and a packaged macOS SDK to produce native Mach-O binaries for x86_64, x86_64h, i386, arm64, and arm64e targets.

## Insight

Reach for OSXCross when you need to **produce macOS artifacts without owning a Mac builder**, typically for:

- **CI release pipelines** that fan out to Linux/Windows/macOS targets — running everything on Linux runners is dramatically cheaper than renting a macOS hosted runner and avoids per-minute pricing on services like GitHub Actions.
- **Universal-binary distribution** for an open-source CLI tool where you want one Linux-based release box producing all the artifacts.
- **Library cross-compile sanity checks** as part of a multi-platform build matrix.

The big caveat is **the macOS SDK**. OSXCross does not (and legally cannot) ship Apple's SDK; you have to extract it yourself from a Command Line Tools or Xcode download obtained under your own Apple developer account. The repo includes scripts to package an SDK tarball from a Mac you own, which you then copy to the Linux host. This is the legal grey zone — fine for personal/internal CI under your own license, less fine if you redistribute the SDK tarball.

Other realities:

- arm64 targets need the macOS 11.0 SDK or newer; arm64e needs a recent Apple Clang.
- Code-signing and notarization still require Apple-only tooling (`codesign`, `notarytool`) — OSXCross gets you a working binary, but distribution to end users still wants a Mac (or a third-party signing service like [rcodesign](https://github.com/indygreg/apple-platform-rs)).
- For Rust specifically, the `cargo-zigbuild` + `zig cc` approach is often easier than maintaining an OSXCross install if you just need `x86_64-apple-darwin` / `aarch64-apple-darwin` targets.

## Similar / related topics

- [[ios]] — the App Store side of Apple toolchains; the iOS SDK has stricter terms than macOS.
- [[osx_tricks]] — the Gatekeeper friction your unsigned cross-built binary will hit on user machines.
- [[utm]] — alternative path: virtualize a real macOS instance for builds (Apple Silicon hosts only, per Apple's EULA).
- [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild) — Zig-based cross-linking for Rust, simpler for Apple targets in many cases.
- [`rcodesign`](https://github.com/indygreg/apple-platform-rs) — Rust reimplementation of Apple code-signing, runnable on Linux.

## Internal links

- [[ios]]
- [[osx_tricks]]
- [[utm]]

## Keywords

`#osxcross` `#cross-compile` `#macos` `#clang` `#cctools` `#ld64` `#sdk` `#ci` `#mach-o`

## References / raw notes

### What it is

> The goal of OSXCross is to provide a well working macOS cross-toolchain for Linux, FreeBSD, OpenBSD, and Android (Termux).
>
> OSXCross works on x86, x86_64, arm and AArch64/arm64, and is able to target arm64, arm64e, x86_64, x86_64h and i386.
>
> arm64 requires the macOS 11.0 SDK (or later). arm64e requires a recent Apple Clang compiler.
>
> There is also a ppc test branch that has recently seen some daylight.

### How it works

To cross-compile for macOS you need:

- the Clang/LLVM compiler
- the cctools (`lipo`, `otool`, `nm`, `ar`, …) and `ld64`
- the macOS SDK

Clang/LLVM is a cross-compiler by default and is now available on nearly every Linux distribution, so the project's job is to provide a proper port of `cctools` / `ld64` and the macOS SDK. OSXCross includes a collection of scripts for preparing the SDK and building `cctools` / `ld64`.

It also includes scripts for optionally building:

- Clang itself using gcc (for distributions that don't include it),
- an up-to-date vanilla GCC as a cross-compiler targeting macOS,
- the `compiler-rt` runtime library, and
- the `llvm-dsymutil` tool required for debugging.

### Repo

<https://github.com/tpoechtrager/osxcross>
