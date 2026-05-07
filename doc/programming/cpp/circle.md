---
title: "Circle — an experimental C++20 compiler with reflection and pattern matching"
main_link: https://www.circle-lang.org/
keywords: [circle, cpp, compiler, reflection, pattern-matching, metaprogramming, sean-baxter, experimental]
status: reviewed
review_date: 2026/05/03
---

# Circle — an experimental C++20 compiler with reflection and pattern matching

**Main link:** <https://www.circle-lang.org/>

Repo: <https://github.com/seanbaxter/circle> · "New Circle" notes: <https://www.circle-lang.org/site/intro/intro.html>

## Summary

Circle is a from-scratch C++20-compatible compiler by Sean Baxter that adds opt-in language extensions which the C++ standard has been promising for decades — full compile-time reflection, pattern matching, choice (sum) types, interfaces and impls, language-level type erasure, a saner declaration syntax (`fn`, `var`), an explicit `forward` keyword, and rich pack-manipulation traits. "New Circle" reframes those features behind a fine-grained per-translation-unit `feature` versioning mechanism, so a project can opt in à la carte while remaining 100% link-compatible with ordinary C++ code.

Often quoted (in this folder originally) as "the best macro language" — because the reflection + pack traits surface lets you do at compile time what you'd reach for a code generator to do in standard C++.

## Insight

Where Circle sits, honestly:

- **It's not a standards-track proposal** — it's one engineer's argument-by-implementation that C++ could already have these features. Some ideas (reflection, pattern matching) are converging with WG21 work for C++26; others (choice types, language type erasure) are unique to Circle.
- **It's not free software in the FSF sense** — the compiler is a proprietary binary you download, not source. That alone disqualifies it for many production codebases regardless of how compelling the features are.
- **It's a single-maintainer project** — there's effectively one person behind it. That's both why it has been so prolific and why it's a real bus-factor risk.
- **100% C++ compatibility is genuinely useful** — you can drop Circle into an existing project and only the files that opt into features change behavior. That makes it cheap to experiment without forking the codebase.

When to actually reach for it:

- Research / prototyping a feature you wish C++ had, before shipping it as a normal C++ library or paper.
- Internal codegen / reflection-heavy systems where the alternative is templated horror or external code generators.
- Teaching / writing — Circle's syntax for sum types and pattern matching is much friendlier than the equivalent `std::variant` + `std::visit` dance.

When to avoid:

- Anything that requires source-level toolchain auditability or a permissive license on the compiler.
- Long-lived production code that needs guaranteed multi-vendor compiler support.
- Codebases that integrate with build systems / IDEs that assume clang/gcc/MSVC.

The realistic alternative for most of the "I want sum types and reflection in my systems language" use case is, frankly, [[programming/rust/README|Rust]]. Circle is the C++-native answer for people who can't or won't switch.

## Similar / related topics

- [Carbon Language](https://github.com/carbon-language/carbon-lang) — Google's "successor" attempt; explicitly bidirectional with C++.
- [Cpp2 / cppfront (Herb Sutter)](https://github.com/hsutter/cppfront) — alternate-syntax front-end that lowers to standard C++.
- [P2996 — Reflection for C++26](https://wg21.link/P2996) — the standards-track reflection proposal Circle has anticipated by years.
- [P1371 — Pattern Matching](https://wg21.link/P1371) — standards-track pattern matching.
- [[programming/rust/README|Rust]] — the obvious "we already have all this" comparison point.

## Internal links

- [[abi]] — sibling: any non-standard compiler raises ABI questions immediately.
- [[programming/cpp/README|C++]] — parent section.
- [[programming/rust/README|Rust]] — the comparison every "but C++ could have..." discussion ends at.

## Keywords

`#circle` `#cpp` `#compiler` `#reflection` `#pattern-matching` `#metaprogramming` `#sean-baxter` `#experimental` `#programming`

## References / raw notes

Originally noted in this vault as: *"Suggested as best macro language."*

From the Circle site:

> Circle is a new C++20 compiler. It's written from scratch and designed for easy extension.
>
> New Circle is a major transformation of the Circle compiler, intended as a response to recent successor language announcements. It focuses on a novel fine-grained versioning mechanism that allows the compiler to fix defects and make the language safer and more productive while maintaining 100% compatibility with existing code assets.

Headline features advertised:

- choice types
- pattern matching
- interfaces and impls
- language type erasure
- as-expressions for safer conversions
- a modern declaration syntax with `fn` and `var` keywords
- a simpler syntax for binary expressions (less operator-precedence surprise)
- a `forward` keyword that takes the bugginess out of forwarding references
- safer initializer lists (resolves `std::initializer_list` ambiguities)
- lifting lambdas to pass overload sets as function arguments
- nine kinds of template parameters
- reflection traits over class types, enum types, function types, class specializations, etc.
- pack traits for pack-transforming algorithms (sort, unique, count, erase, difference, intersection)

The framing pitch: "rather than insisting on a one-size-fits-all approach to language development, project leads can opt into collections of features that best target their projects' needs."
