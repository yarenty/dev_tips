---
title: Brainfuck
main_link: http://www.muppetlabs.com/~breadbox/bf/
keywords: [brainfuck, esolang, turing-complete, urban-mueller, muppetlabs, minimalism]
status: reviewed
review_date: 2026/05/03
---

# Brainfuck

**Main link:** <http://www.muppetlabs.com/~breadbox/bf/>

## Summary

Brainfuck is the minimalist esoteric language created by Urban Müller in 1993 for the Amiga, designed so its compiler could fit in 240 bytes (later <200). It's Turing-complete with just **eight single-character instructions** (`> < + - . , [ ]`) operating on a 30 000-byte tape and a moving pointer. The classic "tiny language" — frequently studied, rarely used.

## Insight

Worth knowing as a **lower bound**: any language richer than Brainfuck is, in principle, doing the same job — Brainfuck shows just how little surface you need to be Turing-complete. Practical uses are mostly pedagogical (teaching pointer mechanics, compilers, JITs, optimisation passes) or recreational (code-golf challenges, esolang demos). When you see a "compiles to Brainfuck" project (e.g. [[brain]]), the interesting part is usually the *front-end* language, not the BF tape — the BF backend is a target so primitive that everyone's optimiser does the same thing.

The eight-instruction core maps cleanly to C, which is why it's a great first compiler-writing exercise.

## Similar / related topics

- [[brain]] — Strongly-typed, Rust-flavoured high-level language that compiles **to** Brainfuck.
- [[algorithms]] — Curated catalogue of common Brainfuck algorithm patterns.
- Whitespace, Befunge, INTERCAL, Malbolge — other classic esolangs in the same minimalism / hostility spirit.
- ELF Kickers (`ebfc`) — toolchain that builds real Linux executables from Brainfuck source.
- Code-golf / esolang community — <https://codegolf.stackexchange.com/>, <https://esolangs.org>.

## Internal links

- [[brain]] — High-level language that targets Brainfuck.
- [[algorithms]] — Catalogue of Brainfuck idioms (input parsing, multiplication, branching).

## Keywords

`#brainfuck` `#esolang` `#turing-complete` `#urban-mueller` `#muppetlabs` `#minimalism`

## References / raw notes

- Project page (Muppetlabs / Brian Raiter): <http://www.muppetlabs.com/~breadbox/bf/>
- Wikipedia: <http://en.wikipedia.org/wiki/Brainfuck>
- esolangs wiki: <https://esolangs.org/wiki/Brainfuck>

### The eight commands

| BF | C equivalent | Meaning |
|----|-------------|---------|
| `>` | `++p;` | Increment the pointer. |
| `<` | `--p;` | Decrement the pointer. |
| `+` | `++*p;` | Increment the byte at the pointer. |
| `-` | `--*p;` | Decrement the byte at the pointer. |
| `.` | `putchar(*p);` | Output the byte at the pointer. |
| `,` | `*p = getchar();` | Read a byte into the cell at the pointer. |
| `[` | `while (*p) {` | Jump past the matching `]` if the byte is zero. |
| `]` | `}` | Jump back to the matching `[` unless the byte is zero. |

Tape model: an array of 30 000 bytes, all initialised to zero, with a single byte-pointer that starts at index 0.

### Notable BF projects (Brian Raiter's site)

- **`bf.asm`** — 166-byte Brainfuck compiler for x86 Linux: <http://www.muppetlabs.com/~breadbox/software/tiny/bf.asm.txt>
- **`quine.b`** — 822-byte Brainfuck self-printing program: <http://www.muppetlabs.com/~breadbox/bf/quine.b.txt>
- **Portable Brainfuck** — informal standards guide for implementers: <http://www.muppetlabs.com/~breadbox/bf/standards.html>
- **ELF Kickers / `ebfc`** — full Brainfuck → ELF compiler, packaged with other ELF-format toys: <http://www.muppetlabs.com/~breadbox/software/elfkickers.html>

### Other implementations of note

- **pibfi (Cat's Eye Tech)** — "Platonic Ideal Brainfuck Interpreter"; can emulate most other interpreters' quirks: <https://catseye.tc/article/Language_Implementations#pibfi>
- **Brainfuck archive (Panu Kalliokoski)** — collection of compilers, interpreters, preprocessors, and example programs: <http://esoteric.sange.fi/brainfuck/>
