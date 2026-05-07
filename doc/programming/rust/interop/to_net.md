---
title: Rust ↔ .NET interop
main_link: https://github.com/FractalFir/rustc_codegen_clr
keywords: [rust, dotnet, clr, csharp, csbindgen, rustc-codegen-clr, ffi, interop]
status: reviewed
---

# Rust ↔ .NET interop

**Main link:** <https://github.com/FractalFir/rustc_codegen_clr>

## Summary

There are two fundamentally different ways to bridge Rust and .NET, and
they answer different questions. **`P/Invoke` + a generator** (the
mainstream path) calls a Rust `cdylib` from C# the same way C# already
calls Win32; the interesting Rust-side tooling here is
[`csbindgen`](https://github.com/Cysharp/csbindgen) by Cysharp, which
auto-generates the C# `[DllImport]` declarations from your Rust function
signatures. **`rustc_codegen_clr`** (FractalFir) is the experimental
opposite extreme: it's a `rustc` codegen backend that emits CIL
(.NET MSIL) bytecode, so Rust crates compile to *.NET assemblies*
runnable on the CLR alongside C# — no FFI boundary at all.

## Insight

Pick by what you actually need:

- **"I have a Rust crate, I want my C# app to call it"** — `csbindgen`.
  Mature, used in production by Cysharp, supports unsafe pointers, native
  AOT, Unity, and `LibraryImport` (the modern source-gen replacement for
  `DllImport`). The other realistic option is hand-written
  `extern "C"` + manual `[DllImport]`, which is fine for small surfaces.
- **"I have a C# library, I want Rust to call it"** — significantly
  trickier. Options: host the CoreCLR via `nethost`/`hostfxr` from a Rust
  process (verbose); use [`netcorehost`](https://crates.io/crates/netcorehost)
  which wraps that for you; or expose the C# code as a UnmanagedCallersOnly
  static method in an AOT-published library and call it via `dlopen`.
- **"I want Rust source compiled to a .NET assembly"** —
  `rustc_codegen_clr`. Experimental research project, but actively
  developed and impressive in scope (preserves types, field names,
  generics). Useful for "embed Rust crates in a Unity / Godot Mono game"
  kinds of scenarios where you'd rather not ship an unmanaged DLL.
- **`cs-bindgen`** (the older Rust-side project, rylev/cs-bindgen) is
  largely dormant; csbindgen has displaced it.

Rough comparison vs the other interop directions:

- **vs Java**: .NET's P/Invoke is dramatically simpler than JNI (no
  per-method `Java_pkg_Class_*` decoration, no JNIEnv plumbing). The
  generator landscape is correspondingly smaller.
- **vs Python**: csbindgen plays the role maturin/PyO3 plays for Python
  — it's the "make my Rust crate consumable by language X" pipeline.
- **NativeAOT angle**: .NET 7+ NativeAOT compiles C# to a static binary
  with no CLR; that makes Rust↔C# look more like Rust↔C++ (just two AOT
  ABIs talking).

## Similar / related topics

- [[pyo3|PyO3]] / [[maturin]] — Python equivalent of csbindgen.
- [[to_java]] — JVM equivalent (JNI / FFM).
- `csbindgen` (Cysharp) — production-grade C# binding generator.
- `cs-bindgen` (rylev) — older, mostly dormant alternative.
- .NET Foreign Function & Memory model + `LibraryImport` source
  generator — the modern C#-side P/Invoke story.

## Internal links

<!-- reviewed -->

- [[pyo3|PyO3]] — Python interop equivalent.
- [[to_java]] — JVM interop equivalent.
- [[README]] — Rust interop landing.

## Keywords

`#rust` `#dotnet` `#clr` `#csharp` `#interop` `#ffi` `#rustc`

## References / raw notes

### `rustc_codegen_clr`

[`rustc_codegen_clr`](https://github.com/FractalFir/rustc_codegen_clr) is
an experimental Rust → .NET compiler backend. It plugs into the `rustc`
codegen API and turns Rust code into .NET assemblies. The translation is
high-level: types, field names, and generics are preserved on the CLR
side, so the resulting assemblies are reasonably idiomatic.

Worth following for the engineering interest alone — FractalFir is also
behind [`jtcpp`](https://github.com/FractalFir/jtcpp) (Java→C++ in Rust)
listed under [[to_java]].

### `csbindgen` (Cysharp)

[`csbindgen`](https://github.com/Cysharp/csbindgen) — the de-facto Rust →
C# binding generator. Generates `[DllImport]` / `LibraryImport` C# stubs
from your Rust `extern "C"` function signatures, including struct
marshalling. Supports Unity and .NET NativeAOT.

### `netcorehost`

[`netcorehost`](https://crates.io/crates/netcorehost) — Rust-side wrapper
around `nethost`/`hostfxr` for booting the CoreCLR from a Rust process
and calling managed methods.
