---
title: windows-rs — Rust for Windows APIs
main_link: https://github.com/microsoft/windows-rs
keywords: [windows, rust, winrt, win32, com, microsoft, ffi]
status: reviewed
---

# windows-rs — Rust for Windows APIs

**Main link:** <https://github.com/microsoft/windows-rs>

## Summary

`windows-rs` is Microsoft's official Rust language projection of the Windows API surface. It exposes the entire Win32 / COM / WinRT API as Rust modules, generated on-the-fly directly from the [Windows metadata](https://github.com/microsoft/win32metadata). Two top-level crates: **`windows`** (safer, idiomatic Rust wrapping of C-style APIs plus full COM/WinRT object model) and **`windows-sys`** (raw, unsafe, low-overhead `extern "system"` bindings for when you only need the C ABI). Sub-crates split out the orthogonal pieces: `windows-core`, `windows-implement`, `windows-interface`, `windows-registry`, `windows-result`, `windows-strings`, `windows-targets`, `windows-version`, plus `cppwinrt` for embedding the C++/WinRT compiler.

## Insight

Reach for `windows-rs` whenever you'd otherwise reach for a Win32 / COM / WinRT API in C++ or C# from Rust — file system internals, registry, NTFS streams, COM automation (Office, Excel, Outlook), DirectX, Windows.Media, Win32 UI (HWND, message loops), Windows runtime sensors, etc. Practical guidance:

- **Pick the right crate.** `windows-sys` for tiny dependencies and matching-the-C-ABI exactly (e.g. allocator hooks, syscalls, panics-in-FFI scenarios). `windows` for everything else — the type-safety, COM lifetime management, and string conversions are worth it.
- **Feature flags select what's compiled in.** `windows` exposes hundreds of API namespaces gated behind Cargo features (e.g. `Win32_System_Registry`, `Win32_Storage_FileSystem`, `UI_Xaml`). Compile times explode if you enable everything; turn on only what you need.
- **String types** are `HSTRING` (WinRT), `PCSTR`/`PCWSTR` (Win32) — convert with `.into()` and the helpers in `windows-strings`.
- **Cross-compiling to Windows from macOS/Linux**: works with [`cargo-xwin`](https://github.com/rust-cross/cargo-xwin) (downloads the MSVC SDK on demand) or [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild) (uses `zig cc` as the linker for the GNU target). MSVC target = `x86_64-pc-windows-msvc`; GNU target = `x86_64-pc-windows-gnu`. The MSVC target is the canonical one Windows ships against.
- **For Windows GUI apps from Rust**: `windows-rs` gives you the raw building blocks (HWND, GDI, Direct2D, WinUI 3); higher-level options live elsewhere — see [[../gui/tauri|tauri]] for cross-platform webview, [[../gui/slint|slint]] for retained GUI, [[../gui/egui|egui]] for immediate-mode.

## Similar / related topics

- [`windows-sys`](https://crates.io/crates/windows-sys) — the raw-FFI sibling, smaller dep tree.
- [`windows-bindgen`](https://crates.io/crates/windows-bindgen) — the metadata-driven binding generator (you can run it yourself for a custom subset).
- [`win32metadata`](https://github.com/microsoft/win32metadata) — Microsoft's machine-readable Win32 metadata (the source these bindings are generated from).
- [C++/WinRT](https://github.com/microsoft/cppwinrt) — the C++ language projection that `windows-rs` follows in spirit.
- [`cargo-xwin`](https://github.com/rust-cross/cargo-xwin) / [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild) — cross-compile Rust → Windows from non-Windows hosts.

## Internal links

- [[../gui/macos|Apple Rust targets]] — sibling story for Apple platforms (windows-rs ↔ objc2 / cacao).
- [[../gui/tauri|tauri]] — cross-platform GUI; uses windows-rs internally on Windows.
- [[../gui/slint|slint]] — retained-mode GUI that targets Windows.
- [[README|Misc Rust]] — parent landing page for this kitchen-sink section.

## Keywords

`#windows` `#rust` `#winrt` `#win32` `#com` `#microsoft` `#ffi`

## References / raw notes

- Repo: <https://github.com/microsoft/windows-rs>
- crates.io: <https://crates.io/crates/windows>, <https://crates.io/crates/windows-sys>
- Sub-crates: `windows`, `windows-bindgen`, `windows-core`, `windows-implement`, `windows-interface`, `windows-registry`, `windows-result`, `windows-strings`, `windows-sys`, `windows-targets`, `windows-version`, `cppwinrt`.
- Cross-compile from macOS/Linux: `cargo-xwin` (MSVC) or `cargo-zigbuild` (GNU).
- Pitch: language projection follows in the tradition of C++/WinRT — natural, idiomatic Rust on top of standardised metadata, not hand-written bindings that drift from the OS.
