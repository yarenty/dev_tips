---
title: delta-rs — Delta Lake from Rust and Python (no Spark)
main_link: https://github.com/delta-io/delta-rs
keywords: [delta, deltalake, delta-rs, lakehouse, acid, parquet, datafusion, rust, python]
status: reviewed
---

# delta-rs — Delta Lake from Rust and Python (no Spark)

**Main link:** <https://github.com/delta-io/delta-rs>

## Summary

**`delta-rs`** is the Rust (and Python, via PyO3) implementation of the [Delta Lake](https://delta.io/) table format — the open-source storage layer originally built by Databricks that adds ACID transactions, schema enforcement, time-travel, and `MERGE` semantics on top of Parquet files. The project's reason to exist is that Delta Lake's reference implementation is JVM-only (Spark + Delta Standalone); `delta-rs` lets non-Spark applications — Rust services, Pandas / Polars / DuckDB pipelines, ad-hoc Python notebooks — read and write Delta tables without dragging in a JVM. It exposes both a low-level Rust API for integrators and a high-level Python `deltalake` package for everyday data work.

## Insight

The project answers the very common practical question: *"my company writes Delta tables from Spark, how do I read them from a Rust API server / a Python script / DuckDB without standing up a Spark cluster?"* The answer is `delta-rs` (Rust) or `pip install deltalake` (Python). The Rust side is also the integration point for engines that want native Delta support: **DataFusion** can register a `DeltaTable` as a table provider, and several Rust analytics tools (Daft, polars-delta) use `delta-rs` under the hood.

How it compares to the alternatives in the open-table-format space:

| Format | Reference impl | Rust SDK | Distinguishing trait |
|---|---|---|---|
| **Delta Lake** | Spark + Delta core | **`delta-rs`** | Best-supported Databricks lineage; JSON commit log |
| **Apache Iceberg** | Java | **`iceberg-rust`** | Hidden partitioning, time-travel by snapshot ID, broader engine support (Trino/Flink/Snowflake) |
| **Apache Hudi** | Spark | (early `hudi-rs`) | Streaming-first, record-level upserts, Uber lineage |
| **LakeSoul** | Rust + Spark/Flink | (project-native) | Newer, China-origin, niche |

Gotchas to know: **concurrent-write semantics** are non-trivial — Delta uses optimistic concurrency on the JSON commit log, and `delta-rs` has had bug-fix history around multi-writer scenarios (see the linked PRs); **protocol version** matters — tables written with newer Delta protocol features (column mapping, deletion vectors, V2 checkpoints) may be partly unsupported by older `delta-rs` releases (see the project's [Protocol Support Level matrix](https://github.com/delta-io/delta-rs?tab=readme-ov-file#protocol-support-level)); the **Python package is the production focus** — the Rust crate's API has churned more than the Python one.

## Similar / related topics

- [[iceberg]] — Apache Iceberg + `iceberg-rust`, the main alternative open-table format.
- [[lakesoul]] — LakeSoul, a newer streaming-first lakehouse format.
- [[vortex]] — a Parquet-replacement file format (different layer: file vs table).
- [[../lance_data_format|lance]] — Lance, columnar format optimised for ML use cases.
- **Apache Hudi** — the third major open table format (record-level upserts, streaming bias).

## Internal links

<!-- reviewed -->

- [[README]] — DataFusion ecosystem landing.
- [[iceberg]] — sibling open-table format article.
- [[lakesoul]] — streaming-batch lakehouse format.
- [[../README|rust/data]] — Rust data section.
- [[../../../../db/formats/README|db/formats]] — broader format landing.

## Keywords

`#delta` `#deltalake` `#delta-rs` `#lakehouse` `#acid` `#parquet` `#datafusion` `#rust` `#python`

## References / raw notes

- Repo: <https://github.com/delta-io/delta-rs>
- Delta Lake project umbrella: <https://github.com/delta-io>
- Protocol support matrix: <https://github.com/delta-io/delta-rs?tab=readme-ov-file#protocol-support-level>
- Concurrent-write PRs (history): <https://github.com/delta-io/delta-rs/pulls?q=concurrent+write>

### Python quickstart (from the repo)

```python
from deltalake import DeltaTable, write_deltalake
import pandas as pd

# write some data into a delta table
df = pd.DataFrame({"id": [1, 2], "value": ["foo", "boo"]})
write_deltalake("./data/delta", df)

# load data from the delta table
dt = DeltaTable("./data/delta")
df2 = dt.to_pandas()

assert df.equals(df2)
```

### Rust equivalent

```rust
use deltalake::{open_table, DeltaTableError};

#[tokio::main]
async fn main() -> Result<(), DeltaTableError> {
    // open the table written from Python
    let table = open_table("./data/delta").await?;

    // show all active files in the table
    let files: Vec<_> = table.get_file_uris()?.collect();
    println!("{:?}", files);

    Ok(())
}
```
