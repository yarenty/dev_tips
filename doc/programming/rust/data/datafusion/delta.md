---
title: DeltaLake
main_link: https://github.com/delta-io/delta-rs
keywords: [delta, datafusion, data, rust, lake, level, processing, apache]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# DeltaLake

**Main link:** <https://github.com/delta-io/delta-rs>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#delta` `#datafusion` `#data` `#rust` `#lake` `#level` `#processing` `#apache`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# DeltaLake

https://github.com/delta-io


https://github.com/delta-io/delta-rs

Delta Lake is an open-source storage format that runs on top of existing data lakes. Delta Lake is compatible with processing engines like Apache Spark and provides benefits such as ACID transaction guarantees, schema enforcement, and scalable data handling.
The Delta Lake project aims to unlock the power of the Deltalake for as many users and projects as possible by providing native low-level APIs aimed at developers and integrators, as well as a high-level operations API that lets you query, inspect, and operate your Delta Lake with ease.


https://github.com/delta-io/delta-rs?tab=readme-ov-file#protocol-support-level


The deltalake library aims to adopt patterns from other libraries in data processing, so getting started should look familiar.

```python
from deltalake import DeltaTable, write_deltalake
import pandas as pd

# write some data into a delta table
df = pd.DataFrame({"id": [1, 2], "value": ["foo", "boo"]})
write_deltalake("./data/delta", df)

# Load data from the delta table
dt = DeltaTable("./data/delta")
df2 = dt.to_pandas()

assert df.equals(df2)
```
The same table can also be loaded using the core Rust crate:
```rust
use deltalake::{open_table, DeltaTableError};

#[tokio::main]
async fn main() -> Result<(), DeltaTableError> {
// open the table written in python
let table = open_table("./data/delta").await?;

    // show all active files in the table
    let files: Vec<_> = table.get_file_uris()?.collect();
    println!("{:?}", files);

    Ok(())
}
```

https://github.com/delta-io/delta-rs/pulls?q=concurrent+write
