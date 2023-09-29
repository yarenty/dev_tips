# Lance

https://github.com/lancedb/lance


Modern columnar data format for ML. Convert from Parquet in 2-lines of code for 100x faster random access, a vector index, data versioning, and more.
Compatible with pandas, DuckDB, Polars, and pyarrow with more integrations on the way.


Lance is a modern columnar data format that is optimized for ML workflows and datasets. Lance is perfect for:

Building search engines and feature stores.
Large-scale ML training requiring high performance IO and shuffles.
Storing, querying, and inspecting deeply nested data for robotics or large blobs like images, point clouds, and more.
The key features of Lance include:

High-performance random access: 100x faster than Parquet without sacrificing scan performance.

Vector search: find nearest neighbors in milliseconds and combine OLAP-queries with vector search.

Zero-copy, automatic versioning: manage versions of your data without needing extra infrastructure.

Ecosystem integrations: Apache Arrow, Pandas, Polars, DuckDB and more on the way.