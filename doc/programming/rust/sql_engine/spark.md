---
title: Spark UDF (and the Rust-on-Spark angle)
main_link: https://spark.apache.org/docs/latest/sql-ref-functions-udf-scalar.html
keywords: [spark, udf, udaf, jvm, jni, blaze, gluten, datafusion, arrow]
status: reviewed
review_date: 2026/05/03
---

# Spark UDF (and the Rust-on-Spark angle)

**Main link:** <https://spark.apache.org/docs/latest/sql-ref-functions-udf-scalar.html>

## Summary

Apache Spark's UDF mechanism lets you extend Spark SQL / DataFrame with custom row-level logic in Scala / Java / Python / R: register a function with `spark.udf().register(...)`, call it in SQL or `df.select(...)`. There are three flavours: **scalar UDF** (one row in, one value out), **UDAF** (`Aggregator[IN, BUF, OUT]` — many rows in, one value out per group), and **UDTF** ("Hive-style" tabular: one row in, many out — only via Hive interop). Spark also supports **integrating Hive UDFs/UDAFs/UDTFs** directly: register a Hive class and call it the same way. From the Rust angle, this section is mostly about (a) the JVM-bridge cost if you really want Rust UDFs in Spark, and (b) the modern alternative — replacing Spark's Java execution layer with Rust ([[../data/datafusion/blaze|Blaze]]) or C++ ([[../data/datafusion/gluten|Gluten]] + Velox) under the same Catalyst plans.

## Insight

Native Spark UDFs in Rust are not a thing — Spark runs on the JVM, and the closest you get is the same JNI bridge described in [[hive]] and [[../interop/to_java]]: write a `cdylib`, expose it as a Java `native` method via `jni-rs`, register it as a Hive-style UDF, pay the per-row JVM↔native cost. That cost is real and almost always defeats the point. The modern, much better story is **don't write Rust Spark UDFs — replace the executor**: Blaze (Kwai), Gluten (Intel + community), Comet (Apple/Apache), and Photon (Databricks, closed) all do the same thing — keep Spark's Catalyst optimiser and SQL frontend, but offload physical execution to a native engine (DataFusion / Velox / ClickHouse) via Substrait or a similar IR, executing on Arrow-shaped columnar batches. That gets you Rust performance for *all* operators (joins, aggregates, expressions), not just one UDF. **Performance gotcha for any Spark UDF (JVM or otherwise)**: row-at-a-time UDFs defeat Tungsten / vectorisation; if you must, prefer Spark's **Pandas UDFs** (Python, Arrow-batched) or Catalyst expressions over hand-rolled UDFs. **Determinism**: UDFs are deterministic by default — call `.asNondeterministic()` if not (Catalyst will pessimistically refuse to push them through joins / aggregations otherwise).

## Similar / related topics

- [[../data/datafusion/blaze|Blaze]] — Kwai's Spark accelerator, swaps JVM execution for DataFusion (Rust).
- [[../data/datafusion/gluten|Gluten]] — Apache incubator project, swaps JVM execution for Velox (C++) or ClickHouse via Substrait.
- **Apache Comet** — Apple's Spark accelerator (incubating); same niche as Blaze, also DataFusion-based.
- [[hive]] — same JVM-bridge cost; Spark integrates Hive UDFs natively.
- [[../interop/to_java]] — `jni-rs` if you really need to call Rust from JVM directly.

## Internal links

- [[README]]
- [[hive]] — same JVM-bridge story; Spark can also load Hive UDFs.
- [[../interop/to_java]] — `jni-rs` mechanics.
- [[../data/datafusion/blaze|Blaze]] / [[../data/datafusion/gluten|Gluten]] — replace JVM execution with native.
- [[../data/datafusion/README|DataFusion landing]]
- [[../../../db/udf/external_udfs|external UDFs survey]]

## Keywords

`#spark` `#udf` `#udaf` `#jvm` `#blaze` `#gluten` `#datafusion` `#arrow`

## References / raw notes

- Scalar UDF reference: <https://spark.apache.org/docs/latest/sql-ref-functions-udf-scalar.html>
- Aggregate UDF reference: <https://spark.apache.org/docs/latest/sql-ref-functions-udf-aggregate.html>
- Hive UDF integration: <https://spark.apache.org/docs/latest/sql-ref-functions-udf-hive.html>
- Tutorial originally linked here: <https://sparkbyexamples.com/spark/spark-sql-udf/>

### Scalar UDF (Java)

```java
import org.apache.spark.sql.*;
import org.apache.spark.sql.api.java.UDF1;
import org.apache.spark.sql.expressions.UserDefinedFunction;
import static org.apache.spark.sql.functions.udf;
import org.apache.spark.sql.types.DataTypes;

SparkSession spark = SparkSession.builder()
  .appName("Java Spark SQL UDF scalar example").getOrCreate();

// Zero-argument non-deterministic UDF
UserDefinedFunction random = udf(() -> Math.random(), DataTypes.DoubleType);
random.asNondeterministic();
spark.udf().register("random", random);
spark.sql("SELECT random()").show();

// One-argument
spark.udf().register("plusOne",
    (UDF1<Integer, Integer>) x -> x + 1, DataTypes.IntegerType);
spark.sql("SELECT plusOne(5)").show();

// UDF in WHERE
spark.udf().register("oneArgFilter",
    (UDF1<Long, Boolean>) x -> x > 5, DataTypes.BooleanType);
spark.range(1, 10).createOrReplaceTempView("test");
spark.sql("SELECT * FROM test WHERE oneArgFilter(id)").show();
```

### Type-safe UDAF via `Aggregator[IN, BUF, OUT]` (Java)

```java
public static class MyAverage extends Aggregator<Employee, Average, Double> {
    public Average zero()                      { return new Average(0L, 0L); }
    public Average reduce(Average b, Employee e) {
        b.setSum(b.getSum() + e.getSalary()); b.setCount(b.getCount() + 1); return b;
    }
    public Average merge(Average b1, Average b2) {
        b1.setSum(b1.getSum() + b2.getSum()); b1.setCount(b1.getCount() + b2.getCount()); return b1;
    }
    public Double finish(Average r)            { return ((double) r.getSum()) / r.getCount(); }
    public Encoder<Average> bufferEncoder()    { return Encoders.bean(Average.class); }
    public Encoder<Double>  outputEncoder()    { return Encoders.DOUBLE(); }
}
```

Then register and call in SQL via the JAR-and-class-name route:

```sql
CREATE FUNCTION myAverage AS 'MyAverage' USING JAR '/tmp/MyAverage.jar';
SELECT myAverage(salary) AS average_salary FROM employees;
```

### Hive UDF integration

```sql
-- Use a stock Hive UDF from Spark SQL
CREATE TEMPORARY FUNCTION testUDF
  AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDFAbs';

SELECT testUDF(value) FROM t;

-- A UDTF
CREATE TEMPORARY FUNCTION hiveUDTF
  AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDTFExplode';

SELECT hiveUDTF(value) FROM t;

-- A UDAF
CREATE TEMPORARY FUNCTION hiveUDAF
  AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDAFSum';

SELECT key, hiveUDAF(value) FROM t GROUP BY key;
```

### Why "Rust UDF in Spark" is usually wrong

UDFs (in any language) are the slowest path through Spark — they break Catalyst's ability to push down predicates, defeat code-gen, and force per-row materialisation. If you are tempted to write a Rust UDF for performance, you are usually solving the wrong problem; the right answers are:

1. Rewrite as a Catalyst expression / SQL — let the optimiser do its job.
2. Use a vectorised Pandas/Arrow UDF (Python, but Arrow-batched).
3. Move the workload off Spark to a native engine (DataFusion, Polars).
4. Keep Spark's plan but swap execution: Blaze / Gluten / Comet.
