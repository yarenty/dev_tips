---
title: "Excel / spreadsheets in Rust (calamine, rust_xlsxwriter, umya-spreadsheet)"
main_link: https://github.com/tafia/calamine
keywords: [rust, excel, xlsx, ods, calamine, rust_xlsxwriter, umya-spreadsheet, spreadsheets]
status: reviewed
review_date: 2026/05/03
---

# Excel / spreadsheets in Rust (calamine, rust_xlsxwriter, umya-spreadsheet)

**Main link:** <https://github.com/tafia/calamine>

## Summary

Rust's spreadsheet ecosystem is split sharply along the **read vs. write** axis. [**calamine**](https://github.com/tafia/calamine) is the de-facto pure-Rust **reader** for `xls`, `xlsx`, `xlsm`, `xlsb`, `xla`, `xlam`, and OpenDocument `.ods`. To **write** new files, you need either [**rust_xlsxwriter**](https://github.com/jmcnamara/rust_xlsxwriter) (Rust port of the very mature Python XlsxWriter, write-only, fastest, well-maintained) or [**umya-spreadsheet**](https://github.com/MathNya/umya-spreadsheet) (read+write+modify-in-place, including formatting/charts/images, much heavier API). Polars and DataFusion both delegate to calamine for Excel ingestion.

## Insight

Decision shortcut:

| What you need | Pick |
|---------------|------|
| Read xlsx / xls / ods → DataFrame or your structs | **calamine** |
| Generate fresh xlsx (reports, exports) | **rust_xlsxwriter** |
| Open existing xlsx, change cells, save back (preserving formatting) | **umya-spreadsheet** |
| You're in Polars / DataFusion already | use their `read_excel` (calamine under the hood) |
| You can shell out to Python | `openpyxl` / `pandas.read_excel` / `XlsxWriter` are still the gold standard |

calamine is the right starting point ~90% of the time — most spreadsheet pipelines are read-only data ingest. It deserializes rows directly into your `#[derive(Deserialize)]` structs with `worksheet.deserialize::<MyRow>()`, which is the killer ergonomic.

When you need to *produce* a workbook (invoices, reports, dashboards), `rust_xlsxwriter` is the clear pick: same author maintains the Python `XlsxWriter` (which the Rust port mirrors), and it has the full feature set (formulas, charts, conditional formatting, images, validation). It's write-only by design.

`umya-spreadsheet` exists for the awkward middle: "open this customer's existing template, fill in 50 cells, save it back, keep all the conditional formatting / company logo / charts intact." That's a different and harder problem; it needs full round-tripping of the OOXML zip's parts. Expect a heavier API and slower throughput than the other two.

Gotchas:

1. **`.xls` (legacy) is read-only territory.** Nothing in pure Rust writes the old binary BIFF format. If you need to *produce* `.xls`, shell out to LibreOffice headless.
2. **Formulas as strings vs. cached values.** Excel files store both the formula text and the last-computed value. calamine returns the cached value by default; if there's no cached value (file wasn't opened in Excel since the formula was added), you get an empty cell.
3. **Date handling.** Excel stores dates as floats with two epoch conventions (1900 vs. 1904); calamine 0.22+ exposes typed `DataType::DateTime`, but older code paths return floats.
4. **Memory on huge files.** calamine reads the whole sheet into memory. For multi-GB exports prefer `xlsx`'s streaming API or pre-convert to CSV/Parquet outside Rust.

## Similar / related topics

- [`rust_xlsxwriter`](https://github.com/jmcnamara/rust_xlsxwriter) — write-only xlsx, Rust port of XlsxWriter; the standard "produce a workbook" library.
- [`umya-spreadsheet`](https://github.com/MathNya/umya-spreadsheet) — full read/write/modify, preserves formatting; heavier API.
- [`xlsxwriter-rs`](https://crates.io/crates/xlsxwriter) — older C-`libxlsxwriter` FFI wrapper; superseded by `rust_xlsxwriter`.
- [`csv`](https://crates.io/crates/csv) crate — for the plain-text alternative when you control the format.
- [Polars](https://pola.rs/) — `read_excel` / `write_excel` use calamine + xlsxwriter under the hood; usually the highest-leverage way to consume spreadsheets in Rust.

## Internal links

- [[programming/rust/io/README|Rust I/O]] — section landing page.
- [[programming/rust/io/json|json]] — sibling format article; same "exchange data with non-Rust people" use case.
- [[excel_udf_in_rust]] — companion article on writing native Excel UDFs from Rust.

## Keywords

`#rust` `#excel` `#xlsx` `#ods` `#calamine` `#rust_xlsxwriter` `#umya-spreadsheet` `#spreadsheets`

## References / raw notes

### calamine — pure-Rust reader

<https://github.com/tafia/calamine>

> An Excel/OpenDocument Spreadsheets file reader / deserializer, in pure Rust.

**Reader only.** Supports:

- Excel binary / XML: `xls`, `xlsx`, `xlsm`, `xlsb`, `xla`, `xlam`
- OpenDocument: `ods`

Deserializes rows directly into structs with `worksheet.deserialize::<MyRow>()` (serde under the hood).

### Writers / round-trippers

| Crate | Read | Write | Modify-in-place | Notes |
|-------|------|-------|-----------------|-------|
| [calamine](https://github.com/tafia/calamine) | ✅ | ❌ | ❌ | Default for ingest |
| [rust_xlsxwriter](https://github.com/jmcnamara/rust_xlsxwriter) | ❌ | ✅ | ❌ | Author also maintains Python XlsxWriter |
| [umya-spreadsheet](https://github.com/MathNya/umya-spreadsheet) | ✅ | ✅ | ✅ | Heavier, but preserves formatting |
| [xlsxwriter](https://crates.io/crates/xlsxwriter) | ❌ | ✅ | ❌ | FFI to libxlsxwriter; superseded |
