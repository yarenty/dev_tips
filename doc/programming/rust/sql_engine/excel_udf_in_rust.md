---
title: Excel UDFs in Rust
main_link: https://crates.io/crates/xladd-derive
keywords: [excel-udf, xladd, rust, xlsx, addin]
status: reviewed
---

# Excel UDFs in Rust

**Main link:** <https://crates.io/crates/xladd-derive>

## Summary

`xladd` (and the proc-macro companion `xladd-derive`) is a niche Rust binding to the **Excel XLL C SDK** — the native plug-in interface that lets you ship a `.xll` shared library containing user-defined Excel functions (UDFs) callable directly from worksheet cells like `=MY_RUST_FN(A1, B1)`. The model: write a normal Rust function, slap a derive macro on it to generate the XLL marshalling, build with the Windows MSVC toolchain (Excel UDFs are Windows-only), and drop the resulting `.xll` into Excel's *Add-Ins* dialog.

## Insight

This is the only interesting "Rust → Excel" path that does not require Office Scripts or VBA. The killer use case is computationally heavy desk-quant / actuarial / risk worksheets where you currently have a giant VBA blob or a C/C++ XLL — Rust gives you both a memory-safety win and an easier build than the canonical Microsoft/Excel-DNA C++ template. Gotchas: **Windows-only** (the XLL ABI does not exist on macOS/Web Excel — for cross-platform UDFs you need Office.js or Office Scripts instead, which are JavaScript); the crate is a thin wrapper, error messages are Excel error codes (`#NUM!`, `#VALUE!`); and you need to think about Excel's calculation model (volatile vs non-volatile, threadsafe declaration). For most "bring data from Rust into Excel" tasks the saner alternatives are the [[../io/xls|`calamine` / `rust_xlsxwriter`]] crates which read/write `.xlsx` files from outside Excel — no add-in install ceremony required.

## Similar / related topics

- **Excel-DNA** (.NET) — by far the most-used XLL-style add-in framework; the C# / F# reference design.
- **Office.js / Office Scripts** — Microsoft's modern cross-platform add-in path; JavaScript/TypeScript only.
- [[../io/xls|`calamine` + `rust_xlsxwriter`]] — read/write Excel files from Rust without going through Excel itself.
- **PyXLL** — Python equivalent of `xladd`; commercial, very polished.
- [[udf_lib|`udf` (MariaDB/MySQL UDFs in Rust)]] — sibling crate from the same author (`pluots`), same "compile to .so/.xll, register with the host" pattern.

## Internal links

<!-- reviewed -->
- [[README]]
- [[../io/xls|Excel files in Rust]] — the file-IO angle if you don't need to live inside Excel.
- [[udf_lib|MariaDB/MySQL UDFs in Rust]] — same author, sibling project.
- [[../misc/windows|Rust on Windows]] — toolchain context.

## Keywords

`#excel-udf` `#xladd` `#xll` `#rust` `#windows` `#addin`

## References / raw notes

- `xladd-derive` proc-macro: <https://crates.io/crates/xladd-derive>
- `xladd`: <https://crates.io/crates/xladd>
- Source: <https://github.com/cmwest/xladd>
- Excel XLL C SDK reference (Microsoft): <https://learn.microsoft.com/en-us/office/client-developer/excel/welcome-to-the-excel-software-development-kit>
- Excel-DNA (the .NET XLL framework worth comparing against): <https://excel-dna.net/>
