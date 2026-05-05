---
title: OSX Cross
main_link: https://github.com/tpoechtrager/osxcross
keywords: [osxcross, osx, tools, sdk, clang, cross, macos]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# OSX Cross

**Main link:** <https://github.com/tpoechtrager/osxcross>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[osx_tricks]] — Jail-break from not opening on OSX _(score 26.8)_
- [[sniffnet]] — sniffnet _(score 23.2)_
- [[dfdx]] — dfdx _(score 15.0)_
- [[utm]] — UTM _(score 14.8)_
- [[ios]] — Ios _(score 14.8)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#osxcross` `#osx` `#tools` `#sdk` `#clang` `#cross` `#macos`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# OSX Cross

https://github.com/tpoechtrager/osxcross


The goal of OSXCross is to provide a well working macOS cross toolchain for
Linux, FreeBSD, OpenBSD, and Android (Termux).

OSXCross works on x86, x86_64, arm and AArch64/arm64,
and is able to target arm64, arm64e, x86_64, x86_64h and i386.

arm64 requires macOS 11.0 SDK (or later).
arm64e requires a recent Apple clang compiler.

There is also a ppc test branch that has recently seen some daylight.

HOW DOES IT WORK?
For cross-compiling for macOS you need

the Clang/LLVM compiler
the cctools (lipo, otool, nm, ar, ...) and ld64
the macOS SDK.
Clang/LLVM is a cross compiler by default and is now available on nearly every Linux distribution, so we just need a proper port of the cctools/ld64 and the macOS SDK.

OSXCross includes a collection of scripts for preparing the SDK and building the cctools/ld64.

It also includes scripts for optionally building

Clang using gcc (for the case your distribution does not include it),
an up-to-date vanilla GCC as a cross-compiler for target macOS,
the "compiler-rt" runtime library, and
the llvm-dsymutil tool required for debugging.
