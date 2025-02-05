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


