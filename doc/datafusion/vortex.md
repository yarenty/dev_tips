# Vortex (2024-10-17)

https://github.com/spiraldb/vortex

Vortex is a toolkit for working with compressed Apache Arrow arrays in-memory, on-disk, and over-the-wire.

Vortex is designed to be to columnar file formats what Apache DataFusion is to query engines (or, analogously, what LLVM + Clang are to compilers): a highly extensible & extremely fast framework for building a modern columnar file format, with a state-of-the-art, "batteries included" reference implementation.

Vortex is an aspiring successor to Apache Parquet, with dramatically faster random access reads (100-200x faster) and scans (2-10x faster), while preserving approximately the same compression ratio and write throughput. It will also support very wide tables (at least 10s of thousands of columns) and (eventually) on-device decompression on GPUs.



The major features of Vortex are:

- Logical Types - a schema definition that makes no assertions about physical layout.
- Zero-Copy to Arrow - "canonicalized" (i.e., fully decompressed) Vortex arrays can be zero-copy converted to/from Apache Arrow arrays.
- Extensible Encodings - a pluggable set of physical layouts. In addition to the builtin set of Arrow-compatible encodings, the Vortex repository includes a number of state-of-the-art encodings (e.g., FastLanes, ALP, FSST, etc.) that are implemented as extensions. While arbitrary encodings can be implemented as extensions, we have intentionally chosen a small set of encodings that are highly data-parallel, which in turn allows for efficient vectorized decoding, random access reads, and (in the future) decompression on GPUs.
- Cascading Compression - data can be recursively compressed with multiple nested encodings.
- Pluggable Compression Strategies - the built-in Compressor is based on BtrBlocks, but other strategies can trivially be used instead.
- Compute - basic compute kernels that can operate over encoded data (e.g., for filter pushdown).
- Statistics - each array carries around lazily computed summary statistics, optionally populated at read-time. These are available to compute kernels as well as to the compressor.
- Serialization - Zero-copy serialization of arrays, both for IPC and for file formats.
- Columnar File Format (in progress) - A modern file format that uses the Vortex serde library to store compressed array data. Optimized for random access reads and extremely fast scans; an aspiring successor to Apache Parquet.