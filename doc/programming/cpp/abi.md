---
title: "C++ ABI (Itanium): name mangling, vtables, and cross-toolchain compatibility"
main_link: https://itanium-cxx-abi.github.io/cxx-abi/abi.html
keywords: [abi, cpp, itanium-abi, name-mangling, libstdcxx, libcxx, vtable, linker]
status: reviewed
---

# C++ ABI (Itanium): name mangling, vtables, and cross-toolchain compatibility

**Main link:** <https://itanium-cxx-abi.github.io/cxx-abi/abi.html>

Related: <https://en.wikipedia.org/wiki/Name_mangling#C++>

## Summary

The "Itanium C++ ABI" is, despite the name, the de-facto C++ ABI on essentially every Unix-like platform — Linux, macOS, BSDs, and bare-metal toolchains. (Windows uses the unrelated MSVC ABI.) It defines how C++ source-level constructs — overloaded function names, namespaces, templates, vtables, RTTI, exceptions, the layout of class members and bases — are lowered into linker symbols and binary layout. It started as the ABI for HP/Intel's now-defunct Itanium architecture, was generalized, and stuck.

## Insight

The ABI is mostly invisible until it's catastrophically not. Reasons it suddenly matters:

- **Linker errors with `_ZN...` symbols** — those are mangled C++ names. `c++filt` (or `llvm-cxxfilt`) decodes them. Mismatch usually means an ODR violation, an inline function defined inconsistently across translation units, or a header/library version skew.
- **Mixing libstdc++ and libc++** — same source, two different standard library implementations with different binary layouts (e.g. `std::string`, `std::list::size()`). Link a `.o` built against one with a `.so` built against the other and you get silent UB or immediate crashes.
- **The std::string COW saga (GCC dual ABI)** — GCC ≥5 changed `std::string` to SSO/non-COW. To stay backward-binary-compatible they introduced the `_GLIBCXX_USE_CXX11_ABI` macro: `0` selects the old COW string with `std::string` mangled into the bare namespace; `1` selects the new one mangled into `std::__cxx11::`. Distros that mix old and new libraries hit this in the form of "undefined reference to `foo(std::__cxx11::string)`".
- **C++ has no stable ABI by design** — unlike C. Adding a virtual function, reordering members, changing template default parameters, or even toggling `-D_GLIBCXX_DEBUG` can change layout. This is why most C++ libraries that care about ABI stability expose a `extern "C"` shim or use the [pImpl idiom](https://en.cppreference.com/w/cpp/language/pimpl).
- **Inter-language FFI** — Rust, Swift, etc. interop with C++ goes via `extern "C"` or via tools like [`cxx`](https://cxx.rs/) and [`autocxx`](https://google.github.io/autocxx/) that generate the mangled symbols correctly.

When to actually open the spec:

- You're writing a tool that needs to mangle/demangle (a debugger, a profiler, a language binding generator).
- You're tracking down a "this works in debug, crashes in release" mystery that smells like layout difference.
- You're shipping a C++ library and want to *avoid* breaking ABI on minor releases — read the [KDE Policies on Binary Compatibility](https://community.kde.org/Policies/Binary_Compatibility_Issues_With_C%2B%2B) which is the most-cited practical guide.

## Similar / related topics

- [Itanium C++ ABI spec](https://itanium-cxx-abi.github.io/cxx-abi/abi.html) — the source of truth.
- [KDE Binary Compatibility Issues with C++](https://community.kde.org/Policies/Binary_Compatibility_Issues_With_C%2B%2B) — the practical "what changes break ABI" cheatsheet.
- [`cxx` (Rust ↔ C++ interop)](https://cxx.rs/) — generates the right mangled symbols on both sides.
- [`c++filt`](https://sourceware.org/binutils/docs/binutils/c_002b_002bfilt.html) / `llvm-cxxfilt` — decoder ring for mangled names.
- [Stable ABI in Python (PEP 384)](https://peps.python.org/pep-0384/) — interesting contrast: how a *managed* runtime carves out a stable ABI subset.

## Internal links

<!-- reviewed -->

- [[circle]] — sibling: experimental C++ extensions; obviously diverge from the standard ABI in interesting ways.
- [[programming/cpp/README|C++]] — parent section.
- [[assembly]] — disassembly is the only way to confirm what a layout actually looks like.
- [[programming/rust/README|Rust]] — Rust's own ABI is also unstable; C ABI is the lingua franca for both.

## Keywords

`#abi` `#cpp` `#itanium-abi` `#name-mangling` `#libstdcxx` `#libcxx` `#vtable` `#linker` `#programming`

## References / raw notes

- Spec: <https://itanium-cxx-abi.github.io/cxx-abi/abi.html>
- Mangling on Wikipedia: <https://en.wikipedia.org/wiki/Name_mangling#C++>
- The `_GLIBCXX_USE_CXX11_ABI` macro and dual-ABI background: <https://gcc.gnu.org/onlinedocs/libstdc++/manual/using_dual_abi.html>
