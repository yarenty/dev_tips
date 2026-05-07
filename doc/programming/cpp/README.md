---
title: "C++"
keywords: [cpp, c-plus-plus, abi, modules, cmake, conan, vcpkg, modern-cpp]
status: reviewed
review_date: 2026/05/03
---

# C++

A small landing for C++ notes. The articles here lean toward the *meta-level* of the language — ABI stability and experimental compiler extensions — rather than how to write a `for` loop in C++23. For the language itself the canonical references far outclass anything I could write here.

## What's actually here

- [[abi]] — the Itanium C++ ABI: name mangling, vtable layout, and the cross-toolchain compatibility story (libstdc++ vs libc++, the std::string COW saga).
- [[circle]] — Sean Baxter's experimental C++20 compiler that adds reflection, pattern matching, choice types, and other "what if C++ had this" features.

## Quick orientation for modern C++

- **Standard versions**: C++20 is the broadly-shipping baseline now (modules, concepts, ranges, coroutines). C++23 added `std::expected`, `std::print`, `std::mdspan`. C++26 is in flight with reflection (finally) and pattern matching as front-runners.
- **Build systems**: CMake remains dominant despite everyone's complaints; [Meson](https://mesonbuild.com/) is the saner sibling; [Bazel](https://bazel.build/) wins at scale; [Buck2](https://buck2.build/) is Meta's open-sourced rewrite.
- **Package managers**: [Conan](https://conan.io/) and [vcpkg](https://vcpkg.io/) are the two real options; both work, neither is loved.
- **Modules**: still rough across compiler/build-system boundaries in 2024-25; clang+CMake is the most usable path.
- **Sanitizers**: ASan/TSan/UBSan/MSan via clang are non-negotiable for any non-trivial C++ codebase.

## Canonical external resources

- [cppreference.com](https://en.cppreference.com/) — the actually-good reference.
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines) — Stroustrup & Sutter's curated "how to use modern C++ well".
- [Compiler Explorer](https://godbolt.org/) — see what the compiler emits.
- [*Effective Modern C++*](https://www.oreilly.com/library/view/effective-modern-c/9781491908419/) — Scott Meyers, the standard "stop writing C++98" book.

## See also

- [[programming/rust/README|Rust]] — the obvious modern alternative; many C++ codebases now consider Rust for new components.
- [[abi]], [[circle]] — sibling articles in this folder.
- [[assembly]] — pairs naturally with C++ for understanding what the compiler actually does.

## Keywords

`#cpp` `#c-plus-plus` `#modern-cpp` `#abi` `#cmake` `#conan` `#vcpkg` `#programming`
