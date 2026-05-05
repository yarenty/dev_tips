# gluten

https://github.com/apache/incubator-gluten?tab=readme-ov-file

Apache Gluten (Incubating): A Middle Layer for Offloading JVM-based SQL Engines' Execution to Native Engines
OpenSSF Best Practices

This project is still under active development now, and doesn't have a stable release. Welcome to evaluate it.

1 Introduction
1.1 Problem Statement
Apache Spark is a stable, mature project that has been developed for many years. It is one of the best frameworks to scale out for processing petabyte-scale datasets. However, the Spark community has had to address performance challenges that require various optimizations over time. As a key optimization in Spark 2.0, Whole Stage Code Generation is introduced to replace Volcano Model, which achieves 2x speedup. Henceforth, most optimizations are at query plan level. Single operator's performance almost stops growing.


On the other side, SQL engines have been researched for many years. There are a few libraries like Clickhouse, Arrow and Velox, etc. By using features like native implementation, columnar data format and vectorized data processing, these libraries can outperform Spark's JVM based SQL engine. However, these libraries only support single node execution.

1.2 Gluten's Solution
“Gluten” is Latin for glue. The main goal of Gluten project is to “glue" native libraries with SparkSQL. Thus, we can benefit from high scalability of Spark SQL framework and high performance of native libraries.

The basic rule of Gluten's design is that we would reuse spark's whole control flow and as many JVM code as possible but offload the compute-intensive data processing part to native code. Here is what Gluten does:

Transform Spark’s whole stage physical plan to Substrait plan and send to native
Offload performance-critical data processing to native library
Define clear JNI interfaces for native libraries
Switch available native backends easily
Reuse Spark’s distributed control flow
Manage data sharing between JVM and native
Extensible to support more native accelerators
1.3 Target User
Gluten's target user is anyone who wants to accelerate SparkSQL fundamentally. As a plugin to Spark, Gluten doesn't require any change for dataframe API or SQL query, but only requires user to make correct configuration. See Gluten configuration properties here.

1.4 References
You can click below links for more related information.

Gluten Intro Video at Data AI Summit 2022
Gluten Intro Article at Medium.com
Gluten Intro Article at Kyligence.io(in Chinese)
Velox Intro from Meta

2 Architecture
The overview chart is like below. Substrait provides a well-defined cross-language specification for data compute operations (see more details here). Spark physical plan is transformed to Substrait plan. Then Substrait plan is passed to native through JNI call. On native side, the native operator chain will be built out and offloaded to native engine. Gluten will return Columnar Batch to Spark and Spark Columnar API (since Spark-3.0) will be used at execution time. Gluten uses Apache Arrow data format as its basic data format, so the returned data to Spark JVM is ArrowColumnarBatch.



Currently, Gluten only supports Clickhouse backend & Velox backend. Velox is a C++ database acceleration library which provides reusable, extensible and high-performance data processing components. More details can be found from https://github.com/facebookincubator/velox/. Gluten can also be extended to support more backends.
There are several key components in Gluten:

Query Plan Conversion: converts Spark's physical plan to Substrait plan.
Unified Memory Management: controls native memory allocation.
Columnar Shuffle: shuffles Gluten columnar data. The shuffle service still reuses the one in Spark core. A kind of columnar exchange operator is implemented to support Gluten columnar data format.
Fallback Mechanism: supports falling back to Vanilla spark for unsupported operators. Gluten ColumnarToRow (C2R) and RowToColumnar (R2C) will convert Gluten columnar data and Spark's internal row data if needed. Both C2R and R2C are implemented in native code as well
Metrics: collected from Gluten native engine to help identify bugs, performance bottlenecks, etc. The metrics are displayed in Spark UI.
Shim Layer: supports multiple Spark versions. We plan to only support Spark's latest 2 or 3 releases. Currently, Spark-3.2, Spark-3.3 & Spark-3.4 (experimental) are supported.
3 How to Use
There are two ways to use Gluten.

3.1 Use Released Jar
One way is to use released jar. Here is a simple example. Currently, only centos7/8 and ubuntu20.04/22.04 are well supported.

spark-shell \
--master yarn --deploy-mode client \
--conf spark.plugins=io.glutenproject.GlutenPlugin \
--conf spark.memory.offHeap.enabled=true \
--conf spark.memory.offHeap.size=20g \
--conf spark.shuffle.manager=org.apache.spark.shuffle.sort.ColumnarShuffleManager \
--jars https://github.com/apache/incubator-gluten/releases/download/v1.0.0/gluten-velox-bundle-spark3.2_2.12-ubuntu_20.04_x86_64-1.0.0.jar
3.2 Custom Build
Alternatively, you can build gluten from source, then do some configurations to enable Gluten plugin for Spark. Here is a simple example. Please refer to the corresponding backend part below for more details.

export gluten_jar = /PATH/TO/GLUTEN/backends-velox/target/<gluten-jar>
spark-shell
--master yarn --deploy-mode client \
--conf spark.plugins=io.glutenproject.GlutenPlugin \
--conf spark.memory.offHeap.enabled=true \
--conf spark.memory.offHeap.size=20g \
--conf spark.driver.extraClassPath=${gluten_jar} \
--conf spark.executor.extraClassPath=${gluten_jar} \
--conf spark.shuffle.manager=org.apache.spark.shuffle.sort.ColumnarShuffleManager
...
3.2.1 Build and install Gluten with Velox backend
If you want to use Gluten Velox backend, see Build with Velox to build and install the necessary libraries.

3.2.2 Build and install Gluten with ClickHouse backend
If you want to use Gluten ClickHouse backend, see Build with ClickHouse Backend. ClickHouse backend is developed by Kyligence, please visit https://github.com/Kyligence/ClickHouse for more infomation.

3.2.3 Build options
See Gluten build guide.

