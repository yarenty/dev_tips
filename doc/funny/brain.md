---
title: brain — Rust-flavoured language that compiles to Brainfuck
main_link: https://github.com/brain-lang/brain
keywords: [brain, brainfuck, esolang, compiler, rust, type-system]
status: reviewed
---

# brain — Rust-flavoured language that compiles to Brainfuck

**Main link:** <https://github.com/brain-lang/brain>

## Summary

`brain` is a strongly-typed, high-level programming language whose compiler — written in Rust — emits Brainfuck. The surface syntax borrows heavily from Rust, so familiar concepts (let bindings, types, control flow) work; the language deviates where Brainfuck's tape model demands it. The point is to make Brainfuck programs writable: instead of hand-crafting `[->+<]` loops you write Rust-style code and let the compiler generate the BF.

## Summary in one line

A Rust-syntax front-end for the Brainfuck VM, with compile-time type-checking.

## Insight

This is mostly a curio rather than a practical tool, but it's a beautifully focused **compiler-construction case study**: the source language is rich enough to be readable, the target is so primitive (8 ops, one tape) that every IR/optimisation choice is visible. Read the source if you want to understand how to map structured types and control flow onto a tape automaton. The compiler itself only targets the language's bundled BF interpreter — generated programs aren't guaranteed to work on arbitrary BF interpreters because they assume specific tape semantics chosen by the brain spec.

Don't reach for it expecting performance or a stable ecosystem.

## Similar / related topics

- [[brainfuck]] — The target VM and ISA that brain compiles to.
- [[algorithms]] — Common Brainfuck idioms, useful when reading brain's compiler output.
- ELF Kickers / `ebfc` — alternative end of the toolchain: compiles BF to native ELF executables.
- LLVM-tutorial-style mini-compilers (e.g. `kaleidoscope`) — same pedagogical category, different target.
- esolang front-ends in general — see <https://esolangs.org>.

## Internal links

<!-- reviewed -->

- [[brainfuck]] — The minimalist target language and its 8-instruction ISA.
- [[algorithms]] — Patterns the compiler relies on (e.g. multiplication, conditional execution).

## Keywords

`#brain` `#brainfuck` `#esolang` `#compiler` `#rust` `#type-system`

## References / raw notes

- Repo: <https://github.com/brain-lang/brain>

### What the project description says

> brain is a strongly-typed, high-level programming language that compiles into brainfuck. Its syntax is based on the Rust programming language (which it is also implemented in). Though many Rust concepts will work in brain, it deviates when necessary in order to better suit the needs of brainfuck programming.

### Key design points (paraphrased from the README)

- **Compile-time type checking.** Many logic errors that pure Brainfuck only surfaces as nonsensical runtime output are caught before the BF is generated.
- **Targets one specific BF interpreter.** The bundled interpreter implements a Brainfuck spec designed for brain's needs (cell width, EOF behaviour, etc.). Generated programs will run there but may not run on every BF interpreter in the wild.
- **The compiler does the boilerplate.** Variable allocation on the tape, pointer arithmetic, multiplication, and control-flow lowering are all handled — the user writes Rust-like code instead of hand-crafted `[…]` loops.
