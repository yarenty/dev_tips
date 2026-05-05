# CDC


## FLink

https://github.com/apache/flink-cdc

Flink CDC is a distributed data integration tool for real time data and batch data. Flink CDC brings the simplicity and elegance of data integration via YAML to describe the data movement and transformation in a Data Pipeline.

The Flink CDC prioritizes efficient end-to-end data integration and offers enhanced functionalities such as full database synchronization, sharding table synchronization, schema evolution and data transformation.

![](https://github.com/apache/flink-cdc/raw/master/docs/static/fig/architecture.png)



## Scylla CDC

https://github.com/scylladb/scylla-cdc-rust?tab=readme-ov-file


Scylla-cdc-rust is a library that enables consuming the Scylla Change Data Capture Log in Rust applications. The library automatically and transparently handles errors and topology changes of the underlying Scylla cluster. Thanks to that, the API allows the user to read the CDC log without having to deeply understand the internal structure of CDC.

It is recommended to get familiar with the documentation of CDC first, in order to understand the concept: https://docs.scylladb.com/using-scylla/cdc/

The library was written in pure Rust, using Scylla Rust Driver and Tokio.



## chgcap

https://github.com/neverchanje/chgcap-rs?tab=readme-ov-file

chgcap - Change-Data-Capture Connectors Library
Chgcap is an open-source library for Change-Data-Capture (CDC) written in Rust. It provides an alternative to Debezium, which is mostly limited to the Java ecosystem. With Chgcap, developers can more easily build custom replicas for their RDBMS. Use cases include creating real-time MySQL caches or real-time OLAP engines for Postgres.

We initially focus on the Rust API, but will consider other language bindings if there are many requests for them.

WARNING: Chgap is currently in its early development phase. When it reaches the beta stage, I will publish a beta version on crates.io. My initial objective is to create a DuckDB+MySQL CDC demo, which will showcase how to create a MySQL replica with OLAP functionality. If this is something you are interested in, welcome to follow us for updates.


### Features
It aims to provide all main features supported by Debezium, including:

Ensures that all data changes are captured.
Produces change events with a very low delay while avoiding increased CPU usage required for frequent polling. For example, for MySQL or PostgreSQL, the delay is in the millisecond range.
Requires no changes to your data model, such as a "Last Updated" column.
Can capture deletes.
Can capture old record state and additional metadata such as transaction ID and causing query, depending on the database’s capabilities and configuration.
Support for loading the initial snapshot before consuming the incremental data.

### Supported Databases

Connector	Databases	Driver
chgcap-mysql	MySQL	mysql_async



## Debazium - JAVA


https://debezium.io/documentation/reference/stable/architecture.html

Debezium Architecture
Most commonly, you deploy Debezium by means of Apache Kafka Connect. Kafka Connect is a framework and runtime for implementing and operating:

Source connectors such as Debezium that send records into Kafka

Sink connectors that propagate records from Kafka topics to other systems

The following image shows the architecture of a change data capture pipeline based on Debezium:



![](https://debezium.io/documentation/reference/stable/_images/debezium-architecture.png)


As shown in the image, the Debezium connectors for MySQL and PostgresSQL are deployed to capture changes to these two types of databases. Each Debezium connector establishes a connection to its source database:

- The MySQL connector uses a client library for accessing the binlog.

- The PostgreSQL connector reads from a logical replication stream.

Kafka Connect operates as a separate service besides the Kafka broker.

By default, changes from one database table are written to a Kafka topic whose name corresponds to the table name. If needed, you can adjust the destination topic name by configuring Debezium’s topic routing transformation. For example, you can:

- Route records to a topic whose name is different from the table’s name

- Stream change event records for multiple tables into a single topic

After change event records are in Apache Kafka, different connectors in the Kafka Connect ecosystem can stream the records to other systems and databases such as Elasticsearch, data warehouses and analytics systems, or caches such as Infinispan. Depending on the chosen sink connector, you might need to configure the Debezium new record state extraction transformation. This Kafka Connect SMT propagates the after structure from a Debezium change event to the sink connector. The modified change event record replaces the original, more verbose record that is propagated by default.

### Debezium Server
Another way to deploy Debezium is by using the Debezium server. The Debezium server is a configurable, ready-to-use application that streams change events from a source database to a variety of messaging infrastructures. The following image shows the architecture of a change data capture pipeline that uses the Debezium Server:



![](https://debezium.io/documentation/reference/stable/_images/debezium-server-architecture.png)

You can configure Debezium server to use one of the Debezium source connectors to capture changes from a source database. Change events can be serialized to different formats like JSON or Apache Avro and then will be sent to one of a variety of messaging infrastructures such as Amazon Kinesis, Google Cloud Pub/Sub, or Apache Pulsar.

### Debezium Engine
Yet an alternative way for using the Debezium connectors is the Debezium engine. In this case, Debezium will not be run via Kafka Connect, but as a library embedded into your custom Java applications. This can be useful for either consuming change events within your application itself, without the needed for deploying complete Kafka and Kafka Connect clusters, or for streaming changes to alternative messaging brokers such as Amazon Kinesis. You can find an example for the latter in the examples repository.


### Source Connectors

    MySQL
    MariaDB
    MongoDB
    PostgreSQL
    Oracle
    SQL Server
    Db2
    Cassandra
    Vitess
    Spanner
    Informix