# Datafusion based projects

2023/01




## Delta RS

https://github.com/delta-io/delta-rs

A native interface to Delta Lake.


This library provides low level access to Delta tables in Rust, which can be used with data processing frameworks like datafusion, ballista, polars, vega, etc. It also provides bindings to other higher level languages such as Python or Ruby.

Features
Supported backends:
- Local file system
- AWS S3
- Azure Blob Storage / Azure Datalake Storage Gen2
- Google Cloud Storage

![](https://github.com/delta-io/delta-rs/raw/main/logo.png)




## ROAPI

https://github.com/roapi/roapi

ROAPI automatically spins up read-only APIs for static datasets without requiring you to write a single line of code. It builds on top of Apache Arrow and Datafusion. The core of its design can be boiled down to the following:

- Query frontends to translate SQL, GraphQL and REST API queries into Datafusion plans.
- Datafusion for query plan execution.
- Data layer to load datasets from a variety of sources and formats with automatic schema inference.
- Response encoding layer to serialize intermediate Arrow record batch into various formats requested by client.

See below for a high level diagram:


![](https://camo.githubusercontent.com/6846b7b40ddfc780fc97c2238fd7a4ea2594bde8883adf30fb0dc8086185c80d/68747470733a2f2f726f6170692e6769746875622e696f2f646f63732f696d616765732f726f6170692e706e67)


## Cube — Headless Business Intelligence

https://github.com/cube-js/cube.js

Cube is the headless business intelligence platform. It helps data engineers and application developers access data from modern data stores, organize it into consistent definitions, and deliver it to every application.

![](https://raw.githubusercontent.com/cube-js/cube.js/master/docs/content/cube-scheme.png)

Learn more about connecting Cube to data sources and analytics & visualization tools.

Cube was designed to work with all SQL-enabled data sources, including cloud data warehouses like Snowflake or Google BigQuery, query engines like Presto or Amazon Athena, and application databases like Postgres. Cube has a built-in relational caching engine to provide sub-second latency and high concurrency for API requests.

For more details, see the introduction page in our documentation.

Why Cube?
If you are building a data application—such as a business intelligence tool or a customer-facing analytics feature—you’ll probably face the following problems:

SQL code organization. Sooner or later, modeling even a dozen metrics with a dozen dimensions using pure SQL queries becomes a maintenance nightmare, which leads to building a modeling framework.
Performance. Most of the time and effort in modern analytics software development is spent providing adequate time to insight. In a world where every company’s data is big data, writing just SQL queries to get insight isn’t enough anymore.
Access Control. It is important to secure and govern access to data for all downstream data consuming applications.
Cube has the necessary infrastructure and features to implement efficient data modeling, access control, and performance optimizations so that every application—like embedded analytics, dashboarding and reporting tools, data notebooks, and other tools—can access consistent data via REST, SQL, and GraphQL APIs.

![](https://raw.githubusercontent.com/cube-js/cube.js/master/docs/content/old-was-vs-cubejs-way.png)



## GreptimeDB

https://github.com/GreptimeTeam/greptimedb


GreptimeDB is an open-source time-series database with a special focus on scalability, analytical capabilities and efficiency. It's designed to work on infrastructure of the cloud era, and users benefit from its elasticity and commodity storage.

Our core developers have been building time-series data platform for years. Based on their best-practices, GreptimeDB is born to give you:

- A standalone binary that scales to highly-available distributed cluster, providing a transparent experience for cluster users
- Optimized columnar layout for handling time-series data; compacted, compressed, stored on various storage backends
- Flexible index options, tackling high cardinality issues down
- Distributed, parallel query execution, leveraging elastic computing resource
- Native SQL, and Python scripting for advanced analytical scenarios
- Widely adopted database protocols and APIs
- Extensible table engine architecture for extensive workloads


NOTES:
- GreptimeDB uses Apache Arrow as the memory model and Apache Parquet as the persistent file format.
- GreptimeDB's query engine is powered by Apache Arrow DataFusion.
- OpenDAL from Datafuse Labs gives GreptimeDB a very general and elegant data access abstraction layer.
- GreptimeDB’s meta service is based on etcd.
- GreptimeDB uses RustPython for experimental embedded python scripting.

### Status
Active!


## CnosDB

https://github.com/cnosdb/cnosdb

CnosDB is An Open Source Distributed Time Series Database with high performance, high compression ratio and high usability.

CnosDB Isipho is an original new version of CnosDB which use Rust, Apache Arrow and DataFusion to build.

Design Objectives of CnosDB2.0
To design and develop a high performance, high compression ratio, highly available, distributed cloud native time series database, which meets the following objectives.

Time Series Database

Extensibility, theoretically support time series without upper limit, completely solve the problem of time series inflation, support horizontal/vertical expansion.
Separate storage and computation. Compute nodes and storage nodes can expand and shrink independently.
High-performance storage and low cost, high-performance I/O stacks, cloud disk and object storage for storage tiering
Query engine supports vectorized queries.
Supports multiple timing protocols to write and query, and provides external components to import data.
Cloud Native

Supports cloud native, making full use of the convenience brought by cloud infrastructure and integrating into cloud native ecology.
High availability, second-level fault recovery, multi-cloud, and multi-zone disaster recovery and preparedness.
Native support multi-tenant, pay-as-you-go.
CDC, logs can be subscribed to and distributed to other nodes.
More configurable items are provided to meet the complex requirements of public cloud users in multiple scenarios.
Cloud edge - end collaboration provides the edge - end integration capability with the public cloud
Converged OLAP/CloudAI data Ecosystem on the cloud.

![CnosDB Architecture](https://github.com/cnosdb/cnosdb/raw/main/docs/source/_static/img/arch.jpg)



## parseable

https://github.com/parseablehq/parseable

Cloud native log observability

Parseable is a lightweight, cloud native log observability engine. It can use either a local drive or S3 (and compatible stores) for backend data storage.

Parseable is written in Rust and uses Apache Arrow and Parquet as underlying data structures. Additionally, it uses a simple, index-free mechanism to organize and query data allowing low latency, and high throughput ingestion and query.

Parseable consumes up to ~80% lower memory and ~50% lower CPU than Elastic for similar ingestion throughput.

🚀 Features
- Choose your own storage backend - local drive or S3 (or compatible) object store.
- Ingestion API compatible with HTTP + JSON output of log agents - Fluentbit ↗︎, Vector ↗︎, Logstash ↗︎ and others.
- Query log data with PostgreSQL compatible SQL.
- Grafana ↗︎ for visualization.
- Auto schema inference (schema evolution coming soon ↗︎).
- Send alerts ↗︎ to webhook targets including Slack.
- Stats API ↗︎ to track ingestion and compressed data.
- Single binary includes all components - ingestion, store and query. Built-in UI.









## QV

https://github.com/timvw/qv 

Quickly view your data (qv)

A simply CLI to quickly view your data. Powered by DataFusion.

Example: View data on local filesystem

qv /mnt/datasets/nyc/green_tripdata_2020-07.csv

Example output:
```log
+----------+----------------------+-----------------------+--------------------+------------+--------------+--------------+-----------------+---------------+-------------+-------+---------+------------+--------------+-----------+-----------------------+--------------+--------------+-----------+----------------------+
| VendorID | lpep_pickup_datetime | lpep_dropoff_datetime | store_and_fwd_flag | RatecodeID | PULocationID | DOLocationID | passenger_count | trip_distance | fare_amount | extra | mta_tax | tip_amount | tolls_amount | ehail_fee | improvement_surcharge | total_amount | payment_type | trip_type | congestion_surcharge |
+----------+----------------------+-----------------------+--------------------+------------+--------------+--------------+-----------------+---------------+-------------+-------+---------+------------+--------------+-----------+-----------------------+--------------+--------------+-----------+----------------------+
| 2        | 2020-07-01 00:05:18  | 2020-07-01 00:22:07   | N                  | 1          | 134          | 35           | 2               | 6.38          | 20.5        | 0.5   | 0.5     | 0          | 0            |           | 0.3                   | 21.8         | 2            | 1         | 0                    |
| 2        | 2020-07-01 00:47:06  | 2020-07-01 00:52:13   | N                  | 1          | 41           | 42           | 1               | 1.06          | 6           | 0.5   | 0.5     | 1.46       | 0            |           | 0.3                   | 8.76         | 1            | 1         | 0                    |
| 2        | 2020-07-01 00:24:59  | 2020-07-01 00:35:18   | N                  | 1          | 42           | 159          | 1               | 2.1           | 9           | 0.5   | 0.5     | 0          | 0            |           | 0.3                   | 10.3         | 2            | 1         | 0                    |
| 2        | 2020-07-01 00:55:12  | 2020-07-01 00:58:45   | N                  | 1          | 116          | 116          | 1               | 0.7           | 5           | 0.5   | 0.5     | 0          | 0            |           | 0.3                   | 6.3          | 2            | 1         | 0                    |
| 2        | 2020-07-01 00:12:36  | 2020-07-01 00:20:14   | N                  | 1          | 43           | 141          | 1               | 1.84          | 8           | 0.5   | 0.5     | 0          | 0            |           | 0.3                   | 12.05        | 2            | 1         | 2.75                 |
| 2        | 2020-07-01 00:30:55  | 2020-07-01 00:37:05   | N                  | 5          | 74           | 262          | 1               | 2.04          | 27          | 0     | 0       | 0          | 0            |           | 0.3                   | 30.05        | 2            | 1         | 2.75                 |
| 2        | 2020-07-01 00:13:00  | 2020-07-01 00:19:09   | N                  | 1          | 159          | 119          | 1               | 1.35          | 6.5         | 0.5   | 0.5     | 0          | 0            |           | 0.3                   | 7.8          | 2            | 1         | 0                    |
| 2        | 2020-07-01 00:39:09  | 2020-07-01 00:40:55   | N                  | 1          | 75           | 75           | 1               | 0.35          | -3.5        | -0.5  | -0.5    | 0          | 0            |           | -0.3                  | -4.8         | 4            | 1         | 0                    |
| 2        | 2020-07-01 00:39:09  | 2020-07-01 00:40:55   | N                  | 1          | 75           | 75           | 1               | 0.35          | 3.5         | 0.5   | 0.5     | 0          | 0            |           | 0.3                   | 4.8          | 2            | 1         | 0                    |
| 2        | 2020-07-01 00:45:59  | 2020-07-01 01:01:02   | N                  | 1          | 75           | 87           | 1               | 8.17          | 24          | 0.5   | 0.5     | 4.21       | 0            |           | 0.3                   | 32.26        | 1            | 1         | 2.75                 |
+----------+----------------------+-----------------------+--------------------+------------+--------------+--------------+-----------------+---------------+-------------+-------+---------+------------+--------------+-----------+-----------------------+--------------+--------------+-----------+----------------------+
```

Features
- View file (and directories of files) contents
- Run SQL against files
- View file schemas
- Supported formats:
  - Deltalake
  - Parquet
  - Avro
  - CSV
  - JSON
- Supported storage sytems:
  - local file system
  - S3 (+ https links from AWS S3 console)
  - GCS




## DataFusion + Substrait

https://github.com/datafusion-contrib/datafusion-substrait#datafusion--substrait

Substrait provides a cross-language serialization format for relational algebra, based on protocol buffers.

This repository provides a Substrait producer and consumer for DataFusion:

The producer converts a DataFusion logical plan into a Substrait protobuf.
The consumer converts a Substrait protobuf into a DataFusion logical plan.
Potential uses of this crate:

Replace the current DataFusion protobuf definition used in Ballista for passing query plan fragments to executors
Make it easier to pass query plans over FFI boundaries, such as from Python to Rust
Allow Apache Calcite query plans to be executed in DataFusion


### Status
ACTIVE



## Dask - SQL  (python)
https://github.com/dask-contrib/dask-sql

dask-sql is a distributed SQL query engine in Python. It allows you to query and transform your data using a mixture of common SQL operations and Python code and also scale up the calculation easily if you need it.

Combine the power of Python and SQL: load your data with Python, transform it with SQL, enhance it with Python and query it with SQL - or the other way round. With dask-sql you can mix the well known Python dataframe API of pandas and Dask with common SQL operations, to process your data in exactly the way that is easiest for you.  
Infinite Scaling: using the power of the great Dask ecosystem, your computations can scale as you need it - from your laptop to your super cluster - without changing any line of SQL code. From k8s to cloud deployments, from batch systems to YARN - if Dask supports it, so will dask-sql.  
Your data - your queries: Use Python user-defined functions (UDFs) in SQL without any performance drawback and extend your SQL queries with the large number of Python libraries, e.g. machine learning, different complicated input formats, complex statistics.  
Easy to install and maintain: dask-sql is just a pip/conda install away (or a docker run if you prefer). No need for complicated cluster setups - dask-sql will run out of the box on your machine and can be easily connected to your computing cluster.  
Use SQL from wherever you like: dask-sql integrates with your jupyter notebook, your normal Python module or can be used as a standalone SQL server from any BI tool. It even integrates natively with Apache Hue.  
GPU Support: dask-sql supports running SQL queries on CUDA-enabled GPUs by utilizing RAPIDS libraries like cuDF, enabling accelerated compute for SQL.


![](https://github.com/dask-contrib/dask-sql/raw/main/.github/animation.gif)

### Status
Active








-----


## DataFusion-CatalogProvider-Glue


Glue as a CatalogProvider for Datafusion.


https://github.com/datafusion-contrib/datafusion-catalogprovider-glue


Output from demo example:
```log
mbpro16(timvw)➜  datafusion-catalogprovider-glue git:(main) ✗ cargo run --example=demo

Compiling datafusion-catalogprovider-glue v0.1.0 (/Users/timvw/src/github/datafusion-catalogprovider-glue)
Finished dev [unoptimized + debuginfo] target(s) in 7.43s
Running `target/debug/examples/demo`
registering tpc-h-parquet-1.customer
+---------------+--------------------+------------+------------+
| table_catalog | table_schema       | table_name | table_type |
+---------------+--------------------+------------+------------+
| glue          | tpc-h-parquet-1    | customer   | BASE TABLE |
| glue          | information_schema | tables     | VIEW       |
| glue          | information_schema | columns    | VIEW       |
| datafusion    | information_schema | tables     | VIEW       |
| datafusion    | information_schema | columns    | VIEW       |
+---------------+--------------------+------------+------------+
+---------------+-----------------+------------+--------------+------------------+----------------+-------------+-----------+--------------------------+------------------------+-------------------+-------------------------+---------------+--------------------+---------------+
| table_catalog | table_schema    | table_name | column_name  | ordinal_position | column_default | is_nullable | data_type | character_maximum_length | character_octet_length | numeric_precision | numeric_precision_radix | numeric_scale | datetime_precision | interval_type |
+---------------+-----------------+------------+--------------+------------------+----------------+-------------+-----------+--------------------------+------------------------+-------------------+-------------------------+---------------+--------------------+---------------+
| glue          | tpc-h-parquet-1 | customer   | c_custkey    | 0                |                | NO          | Int64     |                          |                        |                   |                         |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_name       | 1                |                | YES         | Utf8      |                          | 2147483647             |                   |                         |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_address    | 2                |                | YES         | Utf8      |                          | 2147483647             |                   |                         |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_nationkey  | 3                |                | NO          | Int64     |                          |                        |                   |                         |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_phone      | 4                |                | YES         | Utf8      |                          | 2147483647             |                   |                         |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_acctbal    | 5                |                | NO          | Float64   |                          |                        | 24                | 2                       |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_mktsegment | 6                |                | YES         | Utf8      |                          | 2147483647             |                   |                         |               |                    |               |
| glue          | tpc-h-parquet-1 | customer   | c_comment    | 7                |                | YES         | Utf8      |                          | 2147483647             |                   |                         |               |                    |               |
+---------------+-----------------+------------+--------------+------------------+----------------+-------------+-----------+--------------------------+------------------------+-------------------+-------------------------+---------------+--------------------+---------------+
+-----------+--------------------+---------------------------------------+-------------+-----------------+-----------+--------------+-------------------------------------------------------------------------------------------------------------------+
| c_custkey | c_name             | c_address                             | c_nationkey | c_phone         | c_acctbal | c_mktsegment | c_comment                                                                                                         |
+-----------+--------------------+---------------------------------------+-------------+-----------------+-----------+--------------+-------------------------------------------------------------------------------------------------------------------+
| 1         | Customer#000000001 | IVhzIApeRb ot,c,E                     | 15          | 25-989-741-2988 | 711.56    | BUILDING     | to the even, regular platelets. regular, ironic epitaphs nag e                                                    |
| 2         | Customer#000000002 | XSTf4,NCwDVaWNe6tEgvwfmRchLXak        | 13          | 23-768-687-3665 | 121.65    | AUTOMOBILE   | l accounts. blithely ironic theodolites integrate boldly: caref                                                   |
| 3         | Customer#000000003 | MG9kdTD2WBHm                          | 1           | 11-719-748-3364 | 7498.12   | AUTOMOBILE   | deposits eat slyly ironic, even instructions. express foxes detect slyly. blithely even accounts abov             |
| 4         | Customer#000000004 | XxVSJsLAGtn                           | 4           | 14-128-190-5944 | 2866.83   | MACHINERY    | requests. final, regular ideas sleep final accou                                                                  |
| 5         | Customer#000000005 | KvpyuHCplrB84WgAiGV6sYpZq7Tj          | 3           | 13-750-942-6364 | 794.47    | HOUSEHOLD    | n accounts will have to unwind. foxes cajole accor                                                                |
| 6         | Customer#000000006 | sKZz0CsnMD7mp4Xd0YrBvx,LREYKUWAh yVn  | 20          | 30-114-968-4951 | 7638.57   | AUTOMOBILE   | tions. even deposits boost according to the slyly bold packages. final accounts cajole requests. furious          |
| 7         | Customer#000000007 | TcGe5gaZNgVePxU5kRrvXBfkasDTea        | 18          | 28-190-982-9759 | 9561.95   | AUTOMOBILE   | ainst the ironic, express theodolites. express, even pinto beans among the exp                                    |
| 8         | Customer#000000008 | I0B10bB0AymmC, 0PrRYBCP1yGJ8xcBPmWhl5 | 17          | 27-147-574-9335 | 6819.74   | BUILDING     | among the slyly regular theodolites kindle blithely courts. carefully even theodolites haggle slyly along the ide |
| 9         | Customer#000000009 | xKiAFTjUsCuxfeleNqefumTrjS            | 8           | 18-338-906-3675 | 8324.07   | FURNITURE    | r theodolites according to the requests wake thinly excuses: pending requests haggle furiousl                     |
| 10        | Customer#000000010 | 6LrEaV6KR6PLVcgl2ArL Q3rqzLzcT1 v2    | 5           | 15-741-346-9870 | 2753.54   | HOUSEHOLD    | es regular deposits haggle. fur                                                                                   |
+-----------+--------------------+---------------------------------------+-------------+-----------------+-----------+--------------+-------------------------------------------------------------------------------------------------------------------+
```


### Status
Datfusion 12 - updated 3 months ago




## Flock

Flock: A Low-Cost Streaming Query Engine on FaaS Platforms
CI codecov CLA assistant License: AGPL v3

The generic lambda function code is built in advance and uploaded to AWS S3.

| FaaS Service	| AWS Lambda |	GCP Functions |	Azure Functions |	Architectures	| SIMD |	YSB	| NEXMark
|------|-------|------|--------|--------|--------|---------|-----|
| Flock	| 🏅🏅🏅🏅|	👉 TBD	| 👉 TBD	| Arm, x86 |	✅	| ✅	| ✅|


### Status
Interesting - last release year ago





## Datafusion-Bigtable

https://github.com/datafusion-contrib/datafusion-bigtable

Bigtable data source for Apache Arrow Datafusion

Run SQL on Bigtable
This crate implements Bigtable data source and Executor for Datafusion. It is built on top of gRPC client tonic.

```rust
let bigtable_datasource = BigtableDataSource::new(
"emulator".to_owned(),                               // project
"dev".to_owned(),                                    // instance
"weather_balloons".to_owned(),                       // table
"measurements".to_owned(),                           // column family
vec!["_row_key".to_owned()],                         // table_partition_cols
"#".to_owned(),                                      // table_partition_separator
vec![Field::new("pressure", DataType::Utf8, false)], // qualifiers
true,                                                // only_read_latest
).await.unwrap();

let mut ctx = ExecutionContext::new();
ctx.register_table("weather_balloons", Arc::new(bigtable_datasource)).unwrap();

ctx.sql("SELECT \"_row_key\", pressure, \"_timestamp\" FROM weather_balloons where \"_row_key\" = 'us-west2#3698#2021-03-05-1200'").await?.collect().await?;

```

### Status 
Last updated 6 months ago - datafusion ver 8 !




## Datafusion TUI

https://github.com/datafusion-contrib/datafusion-tui


DataFusion-tui provides an extensible terminal based data analysis tool that uses DataFusion (single node) and Ballista (distributed) as query execution engines. It has drawn inspiration and several features from datafusion-cli. In contrast to datafusion-cli a focus of dft is to provide an interface for leveraging DataFusions extensibility (for example connecting to ObjectStores or querying custom TableProviders).

The objective of dft is to provide users with the experience of having their own local database that allows them to query and join data from disparate data sources all from the terminal.

### Status
7 months - datafusion 8





## Buzz Rust

https://github.com/cloudfuse-io/buzz-rust


Warning This project is a POC and is not actively maintained. It's dependencies are outdated, which might introduce security vulnerabilities.

Buzz is best defined by the following key concepts:

Interactive analytics query engine, it quickly computes statistics or performs searches on huge amounts of data.
Cloud serverless, it does not use any resource when idle, but can scale instantly (and massively) to respond to incoming requests.
Functionally, it can be compared to the managed analytics query services offered by all major cloud providers, but differs in that it is open source and uses lower-level components such as cloud functions and cloud storage.

The current implementation is in Rust and is based on Apache Arrow with the DataFusion engine.

### Status
NOT active. 

