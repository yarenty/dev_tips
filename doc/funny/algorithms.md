---
title: Brainfuck algorithms
main_link: https://esolangs.org/wiki/Brainfuck_algorithms
keywords: [brainfuck, esolang, algorithms, idioms, code-snippets, esolangs-wiki]
status: reviewed
review_date: 2026/05/03
---

# Brainfuck algorithms

**Main link:** <https://esolangs.org/wiki/Brainfuck_algorithms>

## Summary

A community-curated catalogue on the esolangs wiki of common Brainfuck *idioms*: how to clear a cell, copy a value, move data, multiply, add, subtract, do conditionals, take input, print numbers, etc. Because Brainfuck has only 8 instructions, every higher-level operation is a small recipe that you compose by hand — the wiki page is the canonical reference for those recipes.

## Insight

Bookmark this whenever you write Brainfuck *or* a Brainfuck compiler. Even simple-sounding things ("set a cell to a constant", "copy x to y", "if x == 0 then …") have non-obvious tape-shuffling implementations because Brainfuck destroys values it reads. The classic patterns — `[-]` to clear, `[->+<]` to move, `[->+>+<<]` followed by a copy-back to non-destructively duplicate — are the building blocks that any Brainfuck compiler (e.g. [[brain]]) emits in its output. Reading the wiki page is the fastest way to "get" the language.

## Similar / related topics

- [[brainfuck]] — The 8-instruction ISA these algorithms are built on.
- [[brain]] — High-level compiler that emits these patterns mechanically.
- "Programming techniques" page on the same wiki — companion higher-level concepts (data structures, function-call simulation).
- *Brainfuck Programming with C Style* — informal pedagogical writeups; search `bf algorithms tutorial`.
- Code-golf community — alternative algorithms optimised for *source size* rather than tape efficiency.

## Internal links

- [[brainfuck]] — The language whose minimalism makes these patterns necessary.
- [[brain]] — A compiler that *emits* these patterns from higher-level source.

## Keywords

`#brainfuck` `#esolang` `#algorithms` `#idioms` `#code-snippets` `#esolangs-wiki`

## References / raw notes

- esolangs wiki page (canonical): <https://esolangs.org/wiki/Brainfuck_algorithms>

### Common patterns at a glance

| Pattern | What it does |
|---------|--------------|
| `[-]` | Clear cell to zero (destructive). |
| `[->+<]` | Move x to y (destructive on x). |
| `[->+>+<<] >> [-<<+>>]` | Copy x to y, preserving x (uses a temp). |
| `<<<[<<<]` | Find first zero cell to the left. |
| `[>>+<<-]` | Add x into y. |
| `>[-<-<+>>]<<[->>+<<]>` | Subtract y from x. |

(See the wiki for many more, including I/O, multiplication, division, comparison, and decimal printing.)
