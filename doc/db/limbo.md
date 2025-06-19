# Limbo

https://github.com/tursodatabase/limbo

Limbo is a work-in-progress, in-process OLTP database engine library written in Rust that has:

- SQLite compatibility [doc] for SQL dialect, file formats, and the C API
- Language bindings for JavaScript/WebAssembly, Rust, Go, Python, and Java
- Asynchronous I/O support on Linux with io_uring
- OS support for Linux, macOS, and Windows 

In the future, we will be also working on:

- BEGIN CONCURRENT for improved write throughput.
- Indexing for vector search.
- Improved schema management including better ALTER support and strict column types by default.

