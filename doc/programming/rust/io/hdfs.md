# libhdfs3

**Libhdfs3**, designed as an alternative implementation of libhdfs, is implemented based on native Hadoop RPC protocol and HDFS data transfer protocol. It gets rid of the drawbacks of JNI, and it has a lightweight, small memory footprint code base. In addition, it is easy to use and deploy.

The Hadoop Distributed File System (HDFS) is a distributed file system designed to run on commodity hardware. HDFS is highly fault-tolerant and is designed to be deployed on low-cost hardware. HDFS provides high throughput access to application data and is suitable for applications that have large data sets.

HDFS is implemented in JAVA language and additionally provides a JNI based C language library _libhdfs_. To use libhdfs, users must deploy the HDFS jars on every machine. This adds operational complexity for non-Java clients that just want to integrate with HDFS.



Advantages:
- functionality: native access to HDFS file system



Comparative:
libhdfs3 is mature solution at time of choosing solution was only one native available  
- other libraries uses JNI/Java based connectors as so adding operational complexity fo non-Java clients
- Examples:
- [hdfs](https://crates.io/crates/hdfs) JNI
- [fs-hdfs](https://crates.io/crates/fs-hdfs) JNI

Recently (2024-08) few new libraries appeared, which could hava some potential however libhdfs3 is mature project:  
- [hdfs-native](https://crates.io/crates/hdfs-native) - new library in “proof of concept” phase
- 


Importance:
Provides core competetiveness - HDFS support



———
**librdkafka** is a C library implementation of the [Apache Kafka](https://kafka.apache.org/) protocol, providing Producer, Consumer and Admin clients. It was designed with message delivery reliability and high performance in mind, current figures exceed 1 million msgs/second for the producer and 3 million msgs/second for the consumer.



## Advantages:
### Functionality

- Support for all Kafka versions since 0.8.x. For more information about broker compatibility options, check the [librdkafka documentation](https://github.com/edenhill/librdkafka/blob/master/INTRODUCTION.md#broker-version-compatibility).
- Consume from single or multiple topics.
- Automatic consumer rebalancing.
- Customizable rebalance, with pre and post rebalance callbacks.
- Synchronous or asynchronous message production.
- Customizable offset commit.
- Create and delete topics and add and edit partitions.
- Alter broker and topic configurations.
- Access to cluster metadata (list of topic-partitions, replicas, active brokers etc).
- Access to group metadata (list groups, list members of groups, hostnames, etc.).
- Access to producer and consumer metrics, errors and callbacks.
- Exactly-once semantics (EOS) via idempotent and transactional producers and read-committed consumers.
### Performance: millin messages/sec
rust-rdkafka is designed to be easy and safe to use thanks to the abstraction layer written in Rust, while at the same time being extremely fast thanks to the librdkafka C library.
Here are some benchmark results using the [BaseProducer](https://docs.rs/rdkafka/*/rdkafka/producer/base_producer/struct.BaseProducer.html), sending data to a single Kafka 0.11 process running in localhost (default configuration, 3 partitions). Hardware: Dell laptop, with Intel Core i7-4712HQ @ 2.30GHz.
- Scenario: produce 5 million messages, 10 bytes each, wait for all of them to be acked
	- 1045413 messages/s, 9.970 MB/s (average over 5 runs)
- Scenario: produce 100000 messages, 10 KB each, wait for all of them to be acked
	- 24623 messages/s, 234.826 MB/s (average over 5 runs)

Comerative:
rdkafka is mature solution based on well tested rdkafka C library., at time of choosing solution was only one available

importance:

Provides core competetiveness - access to kafka.


———

Datafusion

DataFusion is a very fast, extensible query engine for building high-quality data-centric systems in [Rust](http://rustlang.org/), using the [Apache Arrow](https://arrow.apache.org/) in-memory format.
DataFusion offers SQL and Dataframe APIs, excellent [performance](https://benchmark.clickhouse.com/), built-in support for CSV, Parquet, JSON, and Avro, extensive customization, and a great community.



Functionality:

- Feature-rich [SQL support](https://datafusion.apache.org/user-guide/sql/index.html) and [DataFrame API](https://datafusion.apache.org/user-guide/dataframe.html)
- Blazingly fast, vectorized, multi-threaded, streaming execution engine.
- Native support for Parquet, CSV, JSON, and Avro file formats. Support for custom file formats and non file datasources via the TableProvider trait.
- Many extension points: user defined scalar/aggregate/window functions, DataSources, SQL, other query languages, custom plan and execution nodes, optimizer passes, and more.
- Streaming, asynchronous IO directly from popular object stores, including AWS S3, Azure Blob Storage, and Google Cloud Storage (Other storage systems are supported via the ObjectStore trait).
- [Excellent Documentation](https://docs.rs/datafusion/latest) and a [welcoming community](https://datafusion.apache.org/contributor-guide/communication.html).
- A state of the art query optimizer with expression coercion and simplification, projection and filter pushdown, sort and distribution aware optimizations, automatic join reordering, and more.
- Permissive Apache 2.0 License, predictable and well understood [Apache Software Foundation](https://www.apache.org/) governance.
- Implementation in [Rust](https://www.rust-lang.org/), a modern system language with development productivity similar to Java or Golang, the performance of C++, and [loved by programmers everywhere](https://insights.stackoverflow.com/survey/2021#technology-most-loved-dreaded-and-wanted).
- Support for [Substrait](https://substrait.io/) query plans, to easily pass plans across language and system boundaries.


Comperative

Here are some active projects using DataFusion:
- [Arroyo](https://github.com/ArroyoSystems/arroyo) Distributed stream processing engine in Rust
- [Ballista](https://github.com/apache/datafusion-ballista) Distributed SQL Query Engine
- [CnosDB](https://github.com/cnosdb/cnosdb) Open Source Distributed Time Series Database
- [Comet](https://github.com/apache/datafusion-comet) Apache Spark native query execution plugin
- [Cube Store](https://github.com/cube-js/cube.js/tree/master/rust)
- [Dask SQL](https://github.com/dask-contrib/dask-sql) Distributed SQL query engine in Python
- [delta-rs](https://github.com/delta-io/delta-rs) Native Rust implementation of Delta Lake
- [Exon](https://github.com/wheretrue/exon) Analysis toolkit for life-science applications
- [GlareDB](https://github.com/GlareDB/glaredb) Fast SQL database for querying and analyzing distributed data.
- [GreptimeDB](https://github.com/GreptimeTeam/greptimedb) Open Source & Cloud Native Distributed Time Series Database
- [HoraeDB](https://github.com/apache/incubator-horaedb) Distributed Time-Series Database
- [InfluxDB](https://github.com/influxdata/influxdb) Time Series Database
- [Kamu](https://github.com/kamu-data/kamu-cli/) Planet-scale streaming data pipeline
- [LakeSoul](https://github.com/lakesoul-io/LakeSoul) Open source LakeHouse framework with native IO in Rust.
- [Lance](https://github.com/lancedb/lance) Modern columnar data format for ML
- [ParadeDB](https://github.com/paradedb/paradedb) PostgreSQL for Search & Analytics
- [Parseable](https://github.com/parseablehq/parseable) Log storage and observability platform
- [qv](https://github.com/timvw/qv) Quickly view your data
- [Restate](https://github.com/restatedev) Easily build resilient applications using distributed durable async/await
- [ROAPI](https://github.com/roapi/roapi)
- [Sail](https://github.com/lakehq/sail) Unifying stream, batch, and AI workloads with Apache Spark compatibility
- [Seafowl](https://github.com/splitgraph/seafowl) CDN-friendly analytical database
- [Spice.ai](https://github.com/spiceai/spiceai) Unified SQL query interface & materialization engine
- [Synnada](https://synnada.ai/) Streaming-first framework for data products
- [VegaFusion](https://vegafusion.io/) Server-side acceleration for the [Vega](https://vega.github.io/) visualization grammar
- [ZincObserve](https://github.com/zinclabs/zincobserve) Distributed cloud native observability platform



Importance:
Provides core competiteveness - core library of tachyons.



———
Arrow

Apache Arrow defines a language-independent columnar memory format for flat and hierarchical data, organized for efficient analytic operations on modern hardware like CPUs and GPUs. The Arrow memory format also supports zero-copy reads for lightning-fast data access without serialization overhead.

Arrow’s libraries implement the format and provide building blocks for a range of [use cases](https://arrow.apache.org/use_cases/), including high performance analytics. [Many popular projects](https://arrow.apache.org/powered_by/) use Arrow to ship columnar data efficiently or as the basis for analytic engines.
Libraries are available for [C](https://arrow.apache.org/docs/c_glib/), [C++](https://arrow.apache.org/docs/cpp/), [C#](https://github.com/apache/arrow/blob/main/csharp/README.md), [Go](https://godoc.org/github.com/apache/arrow/go/arrow), [Java](https://arrow.apache.org/docs/java/), [JavaScript](https://arrow.apache.org/docs/js/), [Julia](https://arrow.apache.org/julia/), [MATLAB](https://github.com/apache/arrow/blob/main/matlab/README.md), [Python](https://arrow.apache.org/docs/python/), [R](https://arrow.apache.org/docs/r/), [Ruby](https://github.com/apache/arrow/blob/main/ruby/README.md), and [Rust](https://docs.rs/crate/arrow/).


Advantages:
- language-independent columnar memory format 
- zero-copy reads for lightning-fast data access without serialization overhead
- Arrow consists of a number of connected technologies designed to be integrated into storage and execution engines. The key components of Arrow include:
- **Defined data type sets** including both SQL and JSON types, such as int, BigInt, decimal, varchar, map, struct and array.
- **Canonical representations** are columnar in-memory representations of data to support an arbitrarily complex record structure built on top of the defined data types.
- **Common data structures** arrow-aware companion data structures including pick-lists, hash tables and queues.
- **Inter-process communication** achieved within shared memory, TCP/IP and RDMA.
- **Data libraries** used for reading and writing columnar data in multiple languages, including Java, C++, Python, Ruby, Rust, Go and JavaScript.
- **Pipeline and SIMD algorithms** for various operations including bitmap selection, hashing, filtering, bucketing, sorting and matching.
- **Columnar in-memory compression** including a collection of techniques to increase memory efficiency.
- **Memory persistence tools** for short-term persistence through non-volatile memory, SSD or HDD.

### Performance
The faster a user can get to the answer, the faster they can ask other questions. High performance results in more analysis, more use cases and further innovation. As CPUs become faster and more sophisticated, one of the key challenges is making sure processing technology uses CPUs efficiently.
Arrow is specifically designed to maximize:
- **Cache locality:** Memory buffers are compact representations of data designed for modern CPUs. The structures are defined linearly, matching typical read patterns. That means that data of similar type is co-located in memory. This makes cache prefetching more effective, minimizing CPU stalls resulting from cache misses and main memory accesses. These CPU-efficient data structures and access patterns extend to both traditional flat relational structures and modern complex data structures.
- **Pipelining:** Execution patterns are designed to take advantage of the superscalar and pipelined nature of modern processors. This is done by minimizing in-loop instruction count and loop complexity. These tight loops lead to better performance and less branch-prediction failures.
- **SIMD instructions:** Single Instruction Multiple Data (SIMD) instructions allow execution algorithms to operate more efficiently by executing multiple operations in a single clock cycle. Arrow organizes data to be well-suited for SIMD operations.


Comperative 
Arrow is core part of Datafusion as so Tachyons solution.  There are others while, Arrow is well astablished, mature and constantly growning.


Importance:
Provides core cometetiveness - in-memory format used by al internal components and future connections to additional systems (db / file sources)




-
———
JNI

We use JNI for specific connections to Java based solution that did not have native C or Rust libraries, specifically:
 - Apache Druid
- CarbonData 


Advantages 
Functionality
This project provides complete JNI bindings for Rust, allowing to:
- Implement native Java methods for JVM and Android in Rust
- Call Java code from Rust
- Embed JVM in Rust applications and use any Java libraries


Comperative
JNI is standard to access JVM applications from different  languages.


Importance
Provides core competetivness - access to Java based applications/libraries


——
mimalloc

A drop-in global allocator wrapper around the [mimalloc](https://github.com/microsoft/mimalloc) allocator. Mimalloc is a general purpose, performance oriented allocator built by Microsoft.



Advantages:
mimalloc (pronounced “me-malloc”) is a general purpose allocator with excellent [performance](https://github.com/microsoft/mimalloc#performance) characteristics. Initially developed by Daan Leijen for the runtime systems of the [Koka](https://koka-lang.github.io/) and [Lean](https://github.com/leanprover/lean) languages.


mimalloc is a drop-in replacement for malloc and can be used in other programs without code changes,


- **small and consistent**: the library is about 8k LOC using simple and consistent data structures. This makes it very suitable to integrate and adapt in other projects. For runtime systems it provides hooks for a monotonic _heartbeat_ and deferred freeing (for bounded worst-case times with reference counting). Partly due to its simplicity, mimalloc has been ported to many systems (Windows, macOS, Linux, WASM, various BSD's, Haiku, MUSL, etc) and has excellent support for dynamic overriding. At the same time, it is an industrial strength allocator that runs (very) large scale distributed services on thousands of machines with excellent worst case latencies.
- **free list sharding**: instead of one big free list (per size class) we have many smaller lists per "mimalloc page" which reduces fragmentation and increases locality -- things that are allocated close in time get allocated close in memory. (A mimalloc page contains blocks of one size class and is usually 64KiB on a 64-bit system).
- **free list multi-sharding**: the big idea! Not only do we shard the free list per mimalloc page, but for each page we have multiple free lists. In particular, there is one list for thread-local free operations, and another one for concurrent free operations. Free-ing from another thread can now be a single CAS without needing sophisticated coordination between threads. Since there will be thousands of separate free lists, contention is naturally distributed over the heap, and the chance of contending on a single location will be low -- this is quite similar to randomized algorithms like skip lists where adding a random oracle removes the need for a more complex algorithm.
- **eager page purging**: when a "page" becomes empty (with increased chance due to free list sharding) the memory is marked to the OS as unused (reset or decommitted) reducing (real) memory pressure and fragmentation, especially in long running programs.
- **secure**: _mimalloc_ can be built in secure mode, adding guard pages, randomized allocation, encrypted free lists, etc. to protect against various heap vulnerabilities. The performance penalty is usually around 10% on average over our benchmarks.
- **first-class heaps**: efficiently create and use multiple heaps to allocate across different regions. A heap can be destroyed at once instead of deallocating each object separately.
- **bounded**: it does not suffer from _blowup_ [1], has bounded worst-case allocation times (_wcat_) (upto OS primitives), bounded space overhead (~0.2% meta-data, with low internal fragmentation), and has no internal points of contention using only atomic operations.
- **fast**: In our benchmarks (see [below](https://github.com/microsoft/mimalloc#performance)), _mimalloc_ outperforms other leading allocators (_jemalloc_, _tcmalloc_, _Hoard_, etc), and often uses less memory. A nice property is that it does consistently well over a wide range of benchmarks. There is also good huge OS page support for larger server programs.


Comperative

We tested multiple different allocators:
- snmalloc
- rpmalloc
- tcmalloc
- tikv-jemalloc
- mimalloc


Importance:
And after quite research mimalloc was choosen as most performant and quite mature solution. In some scenarion was seen as taking 50% less memory than others.




——
Async -stream

“Low-level” library
Asynchronous stream of elements.
Provides two macros, stream! and try_stream!, allowing the caller to define asynchronous streams of elements. These are implemented using async & await notation. This crate works without unstable features.

The stream! macro returns an anonymous type implementing the [Stream](https://docs.rs/futures-core/*/futures_core/stream/trait.Stream.html) trait. The Item associated type is the type of the values yielded from the stream. The try_stream! also returns an anonymous type implementing the [Stream](https://docs.rs/futures-core/*/futures_core/stream/trait.Stream.html) trait, but the Item associated type is Result<T, Error>. The try_stream! macro supports using ? notation as part of the implementation.


Advantages
Provides core funstinality for streaming solutions in Rust.

The stream uses a lightweight sender to send values from the stream implementation to the caller. When entering the stream, an Option<T> is stored on the stack. A pointer to the cell is stored in a thread local and poll is called on the async block. When poll returns. sender.send(value) stores the value that cell and yields back to the caller.


Comparative
Core library to asynchronous streaming.


Importance
Provides core competetiveness - tachyons is streaming solution



——
Futures-util

Common utilities and extension traits for the futures-rs library.
Zero-cost asynchronous programming in Rust
futures-rs is a library providing the foundations for asynchronous programming in Rust. It includes key trait definitions like Stream, as well as utilities like join!, select!, and various futures combinator methods which enable expressive asynchronous control flow.

Advantages
Fuctionality - provides acynchronous programming in Rust.
The future defined in the futures-rs crate was the original implementation of the type. To enable the async/await syntax, the core Future trait was moved into Rust’s standard library and became std::future::Future .


Comparative
This is standard /main implementation of Futures in Rust


Importance
Provides core competetivnes -> using functions from it.



——
object_store

A focused, easy to use, idiomatic, high performance, async object store library for interacting with object stores.
Using this crate, the same binary and code can easily run in multiple clouds and local test environments, via a simple runtime configuration change. Supported object stores include:
- [AWS S3](https://aws.amazon.com/s3/)
- [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/)
- [Google Cloud Storage](https://cloud.google.com/storage)
- Local files
- Memory
- [HTTP/WebDAV Storage](https://datatracker.ietf.org/doc/html/rfc2518)
- Custom implementations
Originally developed by [InfluxData](https://www.influxdata.com/) and later donated to [Apache Arrow](https://arrow.apache.org/).


Advantages:

By default, this crate provides the following implementations:
- Memory: `InMemory`
- Local filesystem: `LocalFileSystem`
Feature flags are used to enable support for other implementations:
`	•	gcp`: [Google Cloud Storage](https://cloud.google.com/storage/) support. 
`	•	aws`: [Amazon S3](https://aws.amazon.com/s3/). 
`	•	azure`: [Azure Blob Storage](https://azure.microsoft.com/en-gb/services/storage/blobs/). 
`	•	http`: [HTTP/WebDAV Storage](https://datatracker.ietf.org/doc/html/rfc2518). 

Tachyons solution extend object_store with HDFS (hdfs3_object_store)


Comperative
Access to different types of data in DAtafusion / Tachyons is done through object_store, As so is the  main part of solution.



Importance
Provides core competetiveness - Tachyons solution extend object_store with HDFS (libhdfs3), or Druid / CarbonData 




——
PYO3

[Rust](https://www.rust-lang.org/) bindings for [Python](https://www.python.org/), including tools for creating native Python extension modules. Running and interacting with Python code from a Rust binary is also supported.


Advantages 
Functionality
PyO3 can be used to generate a native Python module. 


- [autopy](https://github.com/autopilot-rs/autopy) _A simple, cross-platform GUI automation library for Python and Rust._
	- Contains an example of building wheels on TravisCI and appveyor using [cibuildwheel](https://github.com/pypa/cibuildwheel)
- [ballista-python](https://github.com/apache/arrow-ballista-python) _A Python library that binds to Apache Arrow distributed query engine Ballista._
- [bed-reader](https://github.com/fastlmm/bed-reader) _Read and write the PLINK BED format, simply and efficiently._
	- Shows Rayon/ndarray::parallel (including capturing errors, controlling thread num), Python types to Rust generics, Github Actions
- [cryptography](https://github.com/pyca/cryptography/tree/main/src/rust) _Python cryptography library with some functionality in Rust._
- [css-inline](https://github.com/Stranger6667/css-inline/tree/master/bindings/python) _CSS inlining for Python implemented in Rust._
- [datafusion-python](https://github.com/apache/arrow-datafusion-python) _A Python library that binds to Apache Arrow in-memory query engine DataFusion._
- [deltalake-python](https://github.com/delta-io/delta-rs/tree/main/python) _Native Delta Lake Python binding based on delta-rs with Pandas integration._
- [fastbloom](https://github.com/yankun1992/fastbloom) _A fast [bloom filter](https://github.com/yankun1992/fastbloom#BloomFilter) | [counting bloom filter](https://github.com/yankun1992/fastbloom#countingbloomfilter) implemented by Rust for Rust and Python!_
- [fastuuid](https://github.com/thedrow/fastuuid/) _Python bindings to Rust’s UUID library._
- [feos](https://github.com/feos-org/feos) _Lightning fast thermodynamic modeling in Rust with fully developed Python interface._
- [forust](https://github.com/jinlow/forust) _A lightweight gradient boosted decision tree library written in Rust._
- [granian](https://github.com/emmett-framework/granian) _A Rust HTTP server for Python applications._
- [greptimedb](https://github.com/GreptimeTeam/greptimedb/tree/main/src/script) _Support [Python scripting](https://docs.greptime.com/user-guide/python-scripts/overview) in the database_
- [haem](https://github.com/BooleanCat/haem) _A Python library for working on Bioinformatics problems._
- [html-py-ever](https://github.com/PyO3/setuptools-rust/tree/main/examples/html-py-ever) _Using [html5ever](https://github.com/servo/html5ever) through [kuchiki](https://github.com/kuchiki-rs/kuchiki) to speed up html parsing and css-selecting._
- [hyperjson](https://github.com/mre/hyperjson) _A hyper-fast Python module for reading/writing JSON data using Rust’s serde-json._
- [inline-python](https://github.com/fusion-engineering/inline-python) _Inline Python code directly in your Rust code._
- [johnnycanencrypt](https://github.com/kushaldas/johnnycanencrypt) OpenPGP library with Yubikey support.
- [jsonschema-rs](https://github.com/Stranger6667/jsonschema-rs/tree/master/bindings/python) _Fast JSON Schema validation library._
- [mocpy](https://github.com/cds-astro/mocpy) _Astronomical Python library offering data structures for describing any arbitrary coverage regions on the unit sphere._
- [opendal](https://github.com/apache/opendal/tree/main/bindings/python) _A data access layer that allows users to easily and efficiently retrieve data from various storage services in a unified way._
- [orjson](https://github.com/ijl/orjson) _Fast Python JSON library._
- [ormsgpack](https://github.com/aviramha/ormsgpack) _Fast Python msgpack library._
- [point-process](https://github.com/ManifoldFR/point-process-rust/tree/master/pylib) _High level API for pointprocesses as a Python library._
- [polaroid](https://github.com/daggy1234/polaroid) _Hyper Fast and safe image manipulation library for Python written in Rust._
- [polars](https://github.com/pola-rs/polars) _Fast multi-threaded DataFrame library in Rust | Python | Node.js._
- [pydantic-core](https://github.com/pydantic/pydantic-core) _Core validation logic for pydantic written in Rust._
- [pyheck](https://github.com/kevinheavey/pyheck) _Fast case conversion library, built by wrapping [heck](https://github.com/withoutboats/heck)._
	- Quite easy to follow as there’s not much code.
- [pyre](https://github.com/Project-Dream-Weaver/pyre-http) _Fast Python HTTP server written in Rust._
- [pyreqwest_impersonate](https://github.com/deedy5/pyreqwest_impersonate) _The fastest python HTTP client that can impersonate web browsers by mimicking their headers and TLS/JA3/JA4/HTTP2 fingerprints._
- [ril-py](https://github.com/Cryptex-github/ril-py) _A performant and high-level image processing library for Python written in Rust._
- [river](https://github.com/online-ml/river) _Online machine learning in python, the computationally heavy statistics algorithms are implemented in Rust._
- [rust-python-coverage](https://github.com/cjermain/rust-python-coverage) _Example PyO3 project with automated test coverage for Rust and Python._
- [tiktoken](https://github.com/openai/tiktoken) _A fast BPE tokeniser for use with OpenAI’s models._
- [tokenizers](https://github.com/huggingface/tokenizers/tree/main/bindings/python) _Python bindings to the Hugging Face tokenizers (NLP) written in Rust._
- [tzfpy](http://github.com/ringsaturn/tzfpy) _A fast package to convert longitude/latitude to timezone name._
- [utiles](https://github.com/jessekrubin/utiles) _Fast Python web-map tile utilities_
- [wasmer-python](https://github.com/wasmerio/wasmer-python) _Python library to run WebAssembly binaries._


Comparative 
Pyo3 is standard solution for binding Rust and Python
mature and well tested - used in lots of libraries exposed in python.



Importance

Provides core competitevness  - expose tachyons in python language. 

 



















