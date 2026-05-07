---
title: "Some Assembly Required: an approachable intro to x86/ARM"
main_link: https://github.com/hackclub/some-assembly-required
keywords: [assembly, x86, arm, low-level, learning, hackclub, tutorial, computer-architecture]
status: reviewed
review_date: 2026/05/03
---

# Some Assembly Required: an approachable intro to x86/ARM

**Main link:** <https://github.com/hackclub/some-assembly-required>

Hosted version: <https://some-assembly-required.com/>

## Summary

A free, community-maintained, ~30-minute introduction to assembly programming aimed at people who have never written it before. Authored under the Hack Club umbrella. It walks through what registers, the stack, and instructions actually are, then gives small runnable examples that work on both Linux and macOS. The depth tops out at "I now understand what my compiler is emitting and can read short snippets" — it is not a substitute for the Intel manuals, but it is the friendliest on-ramp I've found.

## Insight

The reason to bother with assembly today isn't to write programs in it — it's to **stop being mystified by everything below the source language**. After even a brief tour you can read disassembly in a debugger, understand why some C++ template choice exploded into 40 instructions, follow what a perf flame graph is pointing at, or sanity-check what an LLM-generated SIMD intrinsic is actually doing.

Reach for this guide when:

- You're about to start a systems-programming book (CSAPP, *The Elements of Computing Systems*, Operating Systems: Three Easy Pieces) and want a soft warm-up.
- You're tired of `objdump`/Compiler Explorer output looking like noise.
- You want a concrete answer to "why would anyone write a whole game in assembly" — even though, as the author admits, you may finish the guide and still not really understand why Chris Sawyer did that for Rollercoaster Tycoon.

Honest gotchas:

- Linux-and-macOS-only working examples; Windows readers will need to translate calling conventions.
- It barely touches SIMD, vector ISAs, or modern micro-architectural concerns (branch prediction, cache hierarchies). For that, jump to Agner Fog's manuals or *Computer Systems: A Programmer's Perspective*.
- It teaches x86_64 and a touch of ARM — it does not cover RISC-V, which arguably is the cleaner ISA to learn first these days.

## Similar / related topics

- [Compiler Explorer (godbolt.org)](https://godbolt.org/) — paste C/C++/Rust, see the asm. Best companion tool to any assembly guide.
- [Agner Fog's optimization manuals](https://www.agner.org/optimize/) — the deep end: micro-architecture, instruction tables, calling conventions.
- [*Computer Systems: A Programmer's Perspective* (Bryant & O'Hallaron)](https://csapp.cs.cmu.edu/) — the canonical undergrad text that takes you from assembly to OS-level concerns.
- [Easy 6502](https://skilldrick.github.io/easy6502/) — if you want to learn assembly via a simpler, retro ISA before tackling x86.
- [Crafting Interpreters](https://craftinginterpreters.com/) — adjacent: not assembly, but builds intuition for what compilers emit.

## Internal links

- [[programming/rust/README|Rust]] — Rust's `cargo asm` / `cargo show-asm` workflow makes "what does this compile to?" a one-liner.
- [[programming/cpp/README|C++]] — sibling section; ABI/name-mangling notes that complement low-level reading.
- [[programming/unix_systems/README|unix systems]] — once you can read assembly, OS-from-scratch material becomes much less intimidating.
- [[debugger]] — `gdb`/`lldb` disassembly views become useful as soon as you can read them.
- [[trace]] — runtime tracing pairs naturally with low-level visibility.

## Keywords

`#assembly` `#x86` `#arm` `#low-level` `#learning` `#hackclub` `#tutorial` `#computer-architecture` `#programming`

## References / raw notes

From the upstream README:

> An approachable introduction to assembly. Since forever ago, I've wanted to try writing assembly, even if just to understand why the Rollercoaster Tycoon creator would write 99% of the game in it. To be fair, even after all of this, I still don't understand why they did that.
>
> Embarking on this quest, I quickly found a lot of scattered and difficult to understand resources. It took compiling a bunch of different materials together to come to a high level understanding of what's happening in my computer.
>
> I wanted to write down my learnings for those who are new to this part of their computer (like me!), including working code examples. This is by no means an exhaustive guide, but instead serves as an approachable introduction to assembly.
>
> Going through this guide takes as little as 30 minutes, but you can also spend a few hours going through it if you want to delve into the code examples.

Working examples target Linux and macOS.
