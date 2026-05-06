---
title: "Debuggers and dynamic-analysis tools"
main_link: https://valgrind.org/
keywords: [debugger, valgrind, gdb, lldb, rr, memory, profiling, linux, dynamic-analysis]
status: reviewed
---

# Debuggers and dynamic-analysis tools

**Main link:** <https://valgrind.org/>

GDB: <https://www.sourceware.org/gdb/> · LLDB: <https://lldb.llvm.org/> · rr: <https://rr-project.org/>

## Summary

A short picker for the dynamic-analysis tools that come up most often on Linux. Currently centred on Valgrind — a GPL'd suite of dynamic binary instrumentation tools (Memcheck, Helgrind, Cachegrind, Callgrind, Massif, DRD) for finding memory errors, threading bugs, cache misses, and heap profiles in unmodified binaries — but the same slot is filled by GDB/LLDB for interactive debugging, `rr` for record-and-replay, and AddressSanitizer/ThreadSanitizer at compile time.

## Insight

Valgrind's killer feature is *no rebuild required*: just prefix your command with `valgrind` and Memcheck will catch use-after-free, uninitialised reads, leaks, and bad frees. The cost is a 5–100× slowdown, so it's a CI / pre-release tool, not a daily dev loop.

Picker:

- **Memory bugs in C/C++**: Valgrind Memcheck if you can't recompile, otherwise prefer **AddressSanitizer** (`-fsanitize=address`) — orders of magnitude faster, catches more spatial bugs, and well-integrated with clang/gcc.
- **Data races**: Valgrind Helgrind/DRD vs **ThreadSanitizer**. TSan is faster and has fewer false positives; Valgrind wins when you can't rebuild.
- **Interactive stepping / inspect a crash**: GDB on Linux, LLDB on macOS / clang shops.
- **"It only fails sometimes"**: [`rr`](https://rr-project.org/) — record once, replay deterministically, run gdb backwards. Life-changing for race conditions.
- **Cache & hotspot profiling**: Cachegrind/Callgrind (offline, deterministic) or `perf` (low-overhead sampling, kernel-level).

Don't use Valgrind on a JIT-heavy or GPU-heavy workload — it doesn't model GPUs and JITs amplify its overhead.

## Similar / related topics

- [Valgrind](https://valgrind.org/) — Memcheck and friends.
- [GDB](https://www.sourceware.org/gdb/) / [LLDB](https://lldb.llvm.org/) — interactive source-level debuggers.
- [rr](https://rr-project.org/) — record-and-replay debugger; pairs with gdb for reverse execution.
- [AddressSanitizer / ThreadSanitizer](https://github.com/google/sanitizers) — compile-time instrumentation, much faster than Valgrind.
- [`perf`](https://perfwiki.github.io/main/) — Linux's sampling profiler.
- [[trace]] — Trippy and other tracing-flavoured tooling for the network side.

## Internal links

<!-- reviewed -->

- [[trace]]
- [[helix]]
- [[tools/linux/tmux|tmux]]
- [[books]]

## Keywords

`#debugger` `#valgrind` `#gdb` `#lldb` `#rr` `#memcheck` `#sanitizers` `#profiling` `#linux`

## References / raw notes

### Valgrind

Valgrind is a GPL'd system for debugging and profiling Linux programs. With its tool suite you can automatically detect many memory-management and threading bugs and perform detailed profiling.

#### Why use it

- Catches memory-management and threading bugs automatically — saves hours of debugging.
- Detailed profiling tools (Cachegrind, Callgrind, Massif) help locate bottlenecks.
- Free software (GPL) and free of charge.
- Runs on x86/Linux, AMD64/Linux, PPC32/Linux, and most major distros.
- Uses dynamic binary instrumentation — no source modification, recompile, or relink. Just prefix your command with `valgrind`.
- Used on projects from single-user toys up to 25M-LOC codebases.
- Works regardless of source language because it operates on binaries; tools are mostly aimed at C/C++ where bugs are most common, but it has been used with Java, Perl, Python, Fortran, Ada, assembly, etc.
- Gives 100% coverage of user-space code, including system libraries; works on programs you don't have source for.
- Extensible — anyone can write new instrumentation tools. Used in research at Cambridge, MIT, UC Berkeley, CMU, Cornell, ANU, Melbourne, TU München, Graz, and others.
- Actively maintained.

The catch: programs run 5–100× slower under Valgrind. Acceptable as a periodic / CI tool, painful as a daily inner loop.

#### When to use

- **All the time** for short-running programs during development — bugs surface immediately.
- **In automated tests** — unit, integration, system, regression suites under Memcheck mean no code path goes unchecked.
- **After big changes** — guard against newly-introduced memory bugs.
- **When a bug occurs** — get instant feedback on what, where, and why.
- **When a bug is suspected** — strange behaviour? Run it under Memcheck.
- **Before a release** — confidence that the cut is as stable as you can make it.
- **Profiling tools (Cachegrind, Callgrind, Massif)** — whenever you want to understand where time or memory is going.

### macOS

Valgrind on modern macOS is *unmaintained territory*. Prefer:

- **LLDB** for interactive debugging.
- **AddressSanitizer / ThreadSanitizer** via Apple clang.
- **Instruments** (Xcode) for time / allocations profiling.
