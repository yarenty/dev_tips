---
title: Cross-platform game development in Scala (natively)
main_link: https://regblanc.com/blog/cross-platform-game-development-in-scala-natively/
keywords: [scala, scala-native, gamedev, sgl, cross-platform, regblanc]
status: reviewed
---

# Cross-platform game development in Scala (natively)

**Main link:** <https://regblanc.com/blog/cross-platform-game-development-in-scala-natively/>

## Summary

Régis Blanc's blog post (and the **SGL** library it introduces) on building games in Scala that target *both* the JVM (desktop, Android) and Scala Native (iOS, native desktop). The pitch: write your game once in Scala using SGL's small abstraction layer over windowing / input / audio, and let SGL bridge to LWJGL on JVM and to platform-native APIs through Scala Native.

## Insight

Niche but interesting if you already love Scala. The win is using the Scala type system on the JVM for fast iteration and switching to Scala Native for shipping (no JVM in the bundle = small binaries, instant startup, no GC stutter). Caveat: the Scala-Native ecosystem is small, the LWJGL/Scala interop has rough edges, and you're trading the giant Unity/Godot ecosystem for a one-author library. Reach for it as a **personal-project / language-experiment**, not as a way to ship to a large audience. SGL is more interesting as a case study in cross-target Scala than as a production engine.

## Similar / related topics

- **Scala Native** — the LLVM-backed AOT Scala compiler this all rests on: <https://scala-native.org/>
- **LWJGL** — Lightweight Java Game Library, the JVM backend SGL uses: <https://www.lwjgl.org/>
- **libGDX** — Java/Kotlin equivalent that's much more mature and widely shipped.
- **Godot** + GDScript / C# — alternative non-Scala route if you want a real editor.
- **Bevy / Piston** ([[engines]]) — same idea in Rust.

## Internal links

- [[engines]] — Rust counterpart (Bevy / Piston).
- Scala notes — see `programming/scala/README.md` in the vault.

## Keywords

`#scala` `#scala-native` `#gamedev` `#sgl` `#cross-platform` `#regblanc`

## References / raw notes

- Original blog post: <https://regblanc.com/blog/cross-platform-game-development-in-scala-natively/>
- SGL repo (Scala Game Library): <https://github.com/regb/scala-game-library>
- Scala Native: <https://scala-native.org/>
- LWJGL: <https://www.lwjgl.org/>

The blog post walks through the SGL architecture: an `AbstractGame` trait the user implements, then platform-specific `Main` objects compile against either JVM (LWJGL backend) or Scala Native (SDL2 backend). The same game source compiles to both targets with no `#ifdef`-style splitting.
