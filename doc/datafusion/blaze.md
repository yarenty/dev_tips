# blaze

BLAZE
TPC-DS master-ce7-builds

The Blaze accelerator for Apache Spark leverages native vectorized execution to accelerate query processing. It combines the power of the Apache Arrow-DataFusion library and the scale of the Spark distributed computing framework.

Blaze takes a fully optimized physical plan from Spark, mapping it into DataFusion's execution plan, and performs native plan computation in Spark executors.

Blaze is composed of the following high-level components:

Spark Extension: hooks the whole accelerator into Spark execution lifetime.
Spark Shims: specialized codes for different versions of spark.
Native Engine: implements the native engine in rust, including:
ExecutionPlan protobuf specification
JNI gateway
Customized operators, expressions, functions
Based on the inherent well-defined extensibility of DataFusion, Blaze can be easily extended to support:

Various object stores.
Operators.
Simple and Aggregate functions.
File formats.
We encourage you to extend DataFusion capability directly and add the supports in Blaze with simple modifications in plan-serde and extension translation.