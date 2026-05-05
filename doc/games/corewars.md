---
title: Core War
main_link: https://corewar.co.uk/standards.htm
keywords: [corewar, redcode, mars, programming-game, vm, retro, simulation]
status: reviewed
---

# Core War

**Main link:** <https://corewar.co.uk/standards.htm>

## Summary

Core War is the 1984 programming game (D. G. Jones & A. K. Dewdney, *Scientific American*) where two programs written in a tiny assembly language called **Redcode** are loaded into a circular memory space and execute concurrently inside a virtual machine called **MARS** (Memory Array Redcode Simulator). The goal is to crash the opponent — make every one of its instructions raise an illegal-execution fault — without crashing yourself. The whole match runs on as little as 8 KiB of memory and a handful of instructions.

## Insight

A delightful 5-instruction-VM teaching tool: it strips computing down to opcode dispatch, addressing modes, and concurrency. Build a MARS implementation if you want to internalise how `MOV $A, $B` versus `MOV @A, @B` actually feel — the addressing modes (`#` immediate, `$` direct, `@` indirect, `<` `>` predec/postinc) are the same idea you'll meet again in any real ISA. The "ICWS-88" and "ICWS-94" specs are the de-facto standards everyone targets. The game itself is a curio (active community small but persistent, mostly on alt.sys.intel.fcorewar archives), but writing the simulator is genuinely rewarding work.

## Similar / related topics

- [[brainfuck]] — Other tiny-VM programming-as-art exercise; even fewer instructions, no concurrency.
- pMARS — the canonical reference simulator (C, ICWS-88/94 compliant): <https://corewar.co.uk/pmars.htm>
- 7-bit / 8086 emulators in general (DOSBox, MAME) — same satisfaction-loop of building a VM for a tiny ISA.
- Robocode, BattleSnake, AI Battle Arena — the modern "programmable agents fight" genre.
- Z80 / 6502 emulation projects (e.g. [[zxspectrum]]) — sibling retro-computing builds.

## Internal links

<!-- reviewed -->

- [[zxspectrum]] — Sibling retro-emulation project (Z80 / Spectrum 48K).
- [[brainfuck]] — Same "minimal VM for fun" lineage.

## Keywords

`#corewar` `#redcode` `#mars` `#programming-game` `#vm` `#retro` `#simulation`

## References / raw notes

- ICWS-88 / ICWS-94 standards (canonical Redcode spec): <https://corewar.co.uk/standards.htm>
- pMARS reference simulator: <https://corewar.co.uk/pmars.htm>
- Wikipedia: <https://en.wikipedia.org/wiki/Core_War>
- Original *Scientific American* "Computer Recreations" articles by A. K. Dewdney (1984-1987).

### Implementations worth a look

- **MARS (Python, unfinished)** — <https://github.com/richpl/MARS> — pedagogical, ~ICWS-88 subset. Useful as a starting point if you want to write your own.
- **pMARS** — the C reference, runs on essentially every platform; good for sanity-checking your own implementation against.
- **CoreWin / Cwin** — Windows GUIs around pMARS for visual battle viewing.

### Redcode quick reference (subset of the original notes)

Redcode-88 covers ~10 opcodes. The most-used ones:

| Op  | Meaning                                                            |
|-----|--------------------------------------------------------------------|
| DAT | "Dead": data cell. Hitting one kills the executing process.        |
| MOV | Copy A → B.                                                        |
| ADD | B := A + B.                                                        |
| SUB | B := B − A.                                                        |
| JMP | Jump to A.                                                         |
| JMZ | Jump to A if B == 0.                                               |
| JMN | Jump to A if B != 0.                                               |
| DJN | Decrement B; jump to A if B != 0.                                  |
| CMP | Skip next instruction if A == B.                                   |
| SPL | Split: spawn a new process at A; both continue.                    |

Addressing modes:

| Sigil | Mode             | Example                |
|-------|------------------|------------------------|
| `#`   | Immediate        | `MOV #0, #1`           |
| `$`   | Direct (default) | `MOV $A, $B`           |
| `@`   | Indirect (B)     | `MOV $A, @B`           |
| `<`   | Pre-decrement    | `MOV $A, <B`           |
| `>`   | Post-increment   | `MOV $A, >B` (ICWS-94) |

A canonical 1-line warrior — the **Imp**:

```
MOV 0, 1
```

It copies itself one cell forward, every cycle, forever. Hard to kill if your opponent isn't expecting it.
