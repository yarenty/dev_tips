# DeltaLake

https://github.com/delta-io


https://github.com/delta-io/delta-rs

https://github.com/delta-io/delta-rs?tab=readme-ov-file#protocol-support-level


The Delta Lake project aims to unlock the power of the Deltalake for as many users and projects as possible by providing native low-level APIs aimed at developers and integrators, as well as a high-level operations API that lets you query, inspect, and operate your Delta Lake with ease.

Source	Downloads	Installation Command	Docs
PyPi	Downloads	pip install deltalake	Docs
Crates.io	Downloads	cargo add deltalake	Docs
Table of contents
Quick Start
Get Involved
Integrations
Features
Quick Start
The deltalake library aims to adopt patterns from other libraries in data processing, so getting started should look familiar.

from deltalake import DeltaTable, write_deltalake
import pandas as pd

# write some data into a delta table
df = pd.DataFrame({"id": [1, 2], "value": ["foo", "boo"]})
write_deltalake("./data/delta", df)

# Load data from the delta table
dt = DeltaTable("./data/delta")
df2 = dt.to_pandas()

assert df.equals(df2)
The same table can also be loaded using the core Rust crate:

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
You can also try Delta Lake docker at DockerHub | Docker Repo

Get Involved
We encourage you to reach out, and are committed to provide a welcoming community.

Join us in our Slack workspace
Report an issue
Looking to contribute? See our good first issues.
Integrations
Libraries and frameworks that interoperate with delta-rs - in alphabetical order.

AWS SDK for Pandas
ballista
datafusion
dask
datahub
DuckDB
polar
Ray
Features
The following section outlines some core features like supported storage backends and operations that can be performed against tables. The state of implementation of features outlined in the Delta protocol is also tracked.

Cloud Integrations
Storage	Rust	Python	Comment
Local	done	done
S3 - AWS	done	done	requires lock for concurrent writes
S3 - MinIO	done	done	requires lock for concurrent writes
S3 - R2	done	done	No lock required when using AmazonS3ConfigKey::CopyIfNotExists
Azure Blob	done	done
Azure ADLS Gen2	done	done
Microsoft OneLake	done	done
Google Cloud Storage	done	done
Supported Operations
Operation	Rust	Python	Description
Create	done	done	Create a new table
Read	done	done	Read data from a table
Vacuum	done	done	Remove unused files and log entries
Delete - partitions		done	Delete a table partition
Delete - predicates	done	done	Delete data based on a predicate
Optimize - compaction	done	done	Harmonize the size of data file
Optimize - Z-order	done	done	Place similar data into the same file
Merge	done	done	Merge a target Delta table with source data
FS check	done	done	Remove corrupted files from table
Protocol Support Level
Writer Version	Requirement	Status
Version 2	Append Only Tables	done
Version 2	Column Invariants	done
Version 3	Enforce delta.checkpoint.writeStatsAsJson	open
Version 3	Enforcedelta.checkpoint.writeStatsAsStruct	open
Version 3	CHECK constraints	semi-done
Version 4	Change Data Feed
Version 4	Generated Columns
Version 5	Column Mapping
Version 6	Identity Columns
Version 7	Table Features
Reader Version	Requirement	Status
Version 2	Column Mapping
Version 3	Table Features (requires reader V7)




https://github.com/delta-io/delta-rs/pulls?q=concurrent+write


