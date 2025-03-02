# Windows RS

https://github.com/microsoft/windows-rs

## Rust for Windows
The windows and windows-sys crates let you call any Windows API past, present, and future using code generated on the fly directly from the metadata describing the API and right into your Rust package where you can call them as if they were just another Rust module. The Rust language projection follows in the tradition established by C++/WinRT of building language projections for Windows using standard languages and compilers, providing a natural and idiomatic way for Rust developers to call Windows APIs.

This repo is the home of the following crates (and other supporting crates):

- windows - Safer bindings including C-style APIs as well as COM and WinRT APIs.

- windows-bindgen - Windows metadata compiler library.

- windows-core - Type support for the windows crate.

- windows-implement - The implement macro for the windows crate, for implementing COM interfaces.

- windows-interface - The interface macro for the windows crate, for declaring COM interfaces.

- windows-registry - Windows registry.

- windows-result - Windows error handling.

- windows-strings - Windows string types.

- windows-sys - Raw bindings for C-style Windows APIs.

- windows-targets - Import libs for Windows.

- windows-version - Windows version information.

- cppwinrt - Bundles the C++/WinRT compiler for use in Rust.




