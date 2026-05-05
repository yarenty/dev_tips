---
title: Brainfuck
main_link: https://github.com/brain-lang/brain
keywords: [brainfuck, language, programming, brain, programs, rust, compilers]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Brainfuck

**Main link:** <https://github.com/brain-lang/brain>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rust_brain]] — Rust - brain _(score 39.4)_
- [[algorithms]] — Algorithms _(score 24.2)_
- [[circle]] — Circle _(score 15.8)_
- [[derivative]] — Derivative _(score 13.1)_
- [[rtic]] — RTIC _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#brainfuck` `#funny` `#language` `#programming` `#brain` `#programs`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 2 additional top-level heading(s) extracted into sibling files:
> - [Rust - brain](rust_brain.md)
> - [Algorithms](algorithms.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Brainfuck

http://www.muppetlabs.com/~breadbox/bf/

_An Eight-Instruction Turing-Complete Programming Language_

Brainfuck is the ungodly creation of Urban Müller, whose goal was apparently to create a Turing-complete language for which he could write the smallest compiler ever, for the Amiga OS 2.0. His compiler was 240 bytes in size. (Though he improved upon this later -- he informed me at one point that he had managed to bring it under 200 bytes.)

I originally started playing around with Brainfuck because of my own interest in writing very small programs for x86 Linux. I also used it as a vehicle for writing a program that created ELF files. Eventually, however, I too succumbed to the Imp of the Perverse and wrote some actual Brainfuck programs of my own.

The Language
A Brainfuck program has an implicit byte pointer, called "the pointer", which is free to move around within an array of 30000 bytes, initially all set to zero. The pointer itself is initialized to point to the beginning of this array.

The Brainfuck programming language consists of eight commands, each of which is represented as a single character.

```
> 	Increment the pointer.
< 	Decrement the pointer.
+ 	Increment the byte at the pointer.
- 	Decrement the byte at the pointer.
     . 	Output the byte at the pointer.
     , 	Input a byte and store it in the byte at the pointer.
     [ 	Jump forward past the matching ] if the byte at the pointer is zero.
     ] 	Jump backward to the matching [ unless the byte at the pointer is zero.
     The semantics of the Brainfuck commands can also be succinctly expressed in terms of C, as follows (assuming that p has been previously defined as a char*):

> 	becomes 	++p;
< 	becomes 	--p;
+ 	becomes 	++*p;
- 	becomes 	--*p;
     . 	becomes 	putchar(*p);
     , 	becomes 	*p = getchar();
     [ 	becomes 	while (*p) {
     ] 	becomes 	}
     
```     
## Resources
   
- http://www.muppetlabs.com/~breadbox/software/tiny/bf.asm.txt -   A Brainfuck compiler for Linux. This is the final result of my attempt at the tiny-compiler challenge. The executable is 166 bytes in size.
- http://www.muppetlabs.com/~breadbox/bf/quine.b.txt - My own Brainfuck programs include an 822-byte quine, and factor, a program that factors arbitrarily large integers in an arbitrarly large amount of time — and what may have been, at the time that I wrote it, the largest Brainfuck program in existence.
- http://www.muppetlabs.com/~breadbox/bf/standards.html - Portable Brainfuck. My informal standards guide for programmers and implementers alike.
- http://www.muppetlabs.com/~breadbox/software/elfkickers.html -  My ELF Kickers package contains several programs that deal with the ELF file format. Included is ebfc, a real Brainfuck compiler (compiles executables and object files).
     
- https://catseye.tc/article/Language_Implementations#pibfi - pibfi. Cat's Eye Technologies provides pibfi, the Platonic Ideal Brainfuck Interpreter. This interpreter can emulate most if not all other interpreters.
     
- http://esoteric.sange.fi/brainfuck/ - The Brainfuck archive. Panu Kalliokoski provides a collection of Brainfuck compilers, interpreters, preprocessors, and of course extant Brainfuck programs.
- http://en.wikipedia.org/wiki/Brainfuck - Wikipedia's entry on Brainfuck. The entry contains a in-depth description of the language and several basic algorithms, as well as a list of derivative languages.
