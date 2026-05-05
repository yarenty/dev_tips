# Spark UDF



Spark SQL UDF (a.k.a User Defined Function) is the most useful feature of Spark SQL & DataFrame which extends the Spark build in capabilities. In this article, I will explain what is UDF? why do we need it and how to create and using it on DataFrame and SQL using Scala example.

Note: UDF’s are the most expensive operations hence use them only you have no choice and when essential.

What is Spark UDF?
UDF a.k.a User Defined Function, If you are coming from SQL background, UDF’s are nothing new to you as most of the traditional RDBMS databases support User Defined Functions, and Spark UDF’s are similar to these.

In Spark, you create UDF by creating a function in a language you prefer to use for Spark. For example, if you are using Spark with scala, you create a UDF in scala language and wrap it with udf() function or register it as udf to use it on DataFrame and SQL respectively.

Why do we need a Spark UDF?
UDF’s are used to extend the functions of the framework and re-use this function on several DataFrame. For example if you wanted to convert the every first letter of a word in a sentence to capital case, spark build-in features does’t have this function hence you can create it as UDF and reuse this as needed on many Data Frames. UDF’s are once created they can be re-use on several DataFrame’s and SQL expressions.

Before you create any UDF, do your research to check if the similar function you wanted is already available in Spark SQL Functions. Spark SQL provides several predefined common functions and many more new functions are added with every release. hence, It is best to check before you reinventing the wheel.

When you creating UDF’s you need to design them very carefully otherwise you will come across performance issues.

https://sparkbyexamples.com/spark/spark-sql-udf/#:~:text=In%20this%20article%2C%20you%20have,and%20SQL%20(after%20registering)%20.









https://spark.apache.org/docs/latest/sql-ref-functions-udf-scalar.html


## Scalar User Defined Functions (UDFs)
Description
User-Defined Functions (UDFs) are user-programmable routines that act on one row. This documentation lists the classes that are required for creating and registering UDFs. It also contains examples that demonstrate how to define and register UDFs and invoke them in Spark SQL.

UserDefinedFunction
To define the properties of a user-defined function, the user can use some of the methods defined in this class.

asNonNullable(): UserDefinedFunction

Updates UserDefinedFunction to non-nullable.

asNondeterministic(): UserDefinedFunction

Updates UserDefinedFunction to nondeterministic.

withName(name: String): UserDefinedFunction

Updates UserDefinedFunction with a given name.


```java

import org.apache.spark.sql.*;
import org.apache.spark.sql.api.java.UDF1;
import org.apache.spark.sql.expressions.UserDefinedFunction;
import static org.apache.spark.sql.functions.udf;
import org.apache.spark.sql.types.DataTypes;

SparkSession spark = SparkSession
  .builder()
  .appName("Java Spark SQL UDF scalar example")
  .getOrCreate();

// Define and register a zero-argument non-deterministic UDF
// UDF is deterministic by default, i.e. produces the same result for the same input.
UserDefinedFunction random = udf(
  () -> Math.random(), DataTypes.DoubleType
);
random.asNondeterministic();
spark.udf().register("random", random);
spark.sql("SELECT random()").show();
// +-------+
// |UDF()  |
// +-------+
// |xxxxxxx|
// +-------+

// Define and register a one-argument UDF
spark.udf().register("plusOne",
  (UDF1<Integer, Integer>) x -> x + 1, DataTypes.IntegerType);
spark.sql("SELECT plusOne(5)").show();
// +----------+
// |plusOne(5)|
// +----------+
// |         6|
// +----------+

// Define and register a two-argument UDF
UserDefinedFunction strLen = udf(
  (String s, Integer x) -> s.length() + x, DataTypes.IntegerType
);
spark.udf().register("strLen", strLen);
spark.sql("SELECT strLen('test', 1)").show();
// +------------+
// |UDF(test, 1)|
// +------------+
// |           5|
// +------------+

// UDF in a WHERE clause
spark.udf().register("oneArgFilter",
  (UDF1<Long, Boolean>) x -> x > 5, DataTypes.BooleanType);
spark.range(1, 10).createOrReplaceTempView("test");
spark.sql("SELECT * FROM test WHERE oneArgFilter(id)").show();
// +---+
// | id|
// +---+
// |  6|
// |  7|
// |  8|
// |  9|
// +---+

```



## User Defined Aggregate Functions (UDAFs)


https://spark.apache.org/docs/latest/sql-ref-functions-udf-aggregate.html



Description
User-Defined Aggregate Functions (UDAFs) are user-programmable routines that act on multiple rows at once and return a single aggregated value as a result. This documentation lists the classes that are required for creating and registering UDAFs. It also contains examples that demonstrate how to define and register UDAFs in Scala and invoke them in Spark SQL.

Aggregator[-IN, BUF, OUT]
A base class for user-defined aggregations, which can be used in Dataset operations to take all of the elements of a group and reduce them to a single value.

IN - The input type for the aggregation.

BUF - The type of the intermediate value of the reduction.

OUT - The type of the final output result.

bufferEncoder: Encoder[BUF]

Specifies the Encoder for the intermediate value type.

finish(reduction: BUF): OUT

Transform the output of the reduction.

merge(b1: BUF, b2: BUF): BUF

Merge two intermediate values.

outputEncoder: Encoder[OUT]

Specifies the Encoder for the final output value type.

reduce(b: BUF, a: IN): BUF

Aggregate input value a into current intermediate value. For performance, the function may modify b and return it instead of constructing new object for b.

zero: BUF

The initial value of the intermediate result for this aggregation.

Examples
Type-Safe User-Defined Aggregate Functions
User-defined aggregations for strongly typed Datasets revolve around the Aggregator abstract class. For example, a type-safe user-defined average can look like:


```java
import java.io.Serializable;

import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Encoder;
import org.apache.spark.sql.Encoders;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.TypedColumn;
import org.apache.spark.sql.expressions.Aggregator;

public static class Employee implements Serializable {
  private String name;
  private long salary;

  // Constructors, getters, setters...

}

public static class Average implements Serializable  {
  private long sum;
  private long count;

  // Constructors, getters, setters...

}

public static class MyAverage extends Aggregator<Employee, Average, Double> {
  // A zero value for this aggregation. Should satisfy the property that any b + zero = b
  public Average zero() {
    return new Average(0L, 0L);
  }
  // Combine two values to produce a new value. For performance, the function may modify `buffer`
  // and return it instead of constructing a new object
  public Average reduce(Average buffer, Employee employee) {
    long newSum = buffer.getSum() + employee.getSalary();
    long newCount = buffer.getCount() + 1;
    buffer.setSum(newSum);
    buffer.setCount(newCount);
    return buffer;
  }
  // Merge two intermediate values
  public Average merge(Average b1, Average b2) {
    long mergedSum = b1.getSum() + b2.getSum();
    long mergedCount = b1.getCount() + b2.getCount();
    b1.setSum(mergedSum);
    b1.setCount(mergedCount);
    return b1;
  }
  // Transform the output of the reduction
  public Double finish(Average reduction) {
    return ((double) reduction.getSum()) / reduction.getCount();
  }
  // Specifies the Encoder for the intermediate value type
  public Encoder<Average> bufferEncoder() {
    return Encoders.bean(Average.class);
  }
  // Specifies the Encoder for the final output value type
  public Encoder<Double> outputEncoder() {
    return Encoders.DOUBLE();
  }
}

Encoder<Employee> employeeEncoder = Encoders.bean(Employee.class);
String path = "examples/src/main/resources/employees.json";
Dataset<Employee> ds = spark.read().json(path).as(employeeEncoder);
ds.show();
// +-------+------+
// |   name|salary|
// +-------+------+
// |Michael|  3000|
// |   Andy|  4500|
// | Justin|  3500|
// |  Berta|  4000|
// +-------+------+

MyAverage myAverage = new MyAverage();
// Convert the function to a `TypedColumn` and give it a name
TypedColumn<Employee, Double> averageSalary = myAverage.toColumn().name("average_salary");
Dataset<Double> result = ds.select(averageSalary);
result.show();
// +--------------+
// |average_salary|
// +--------------+
// |        3750.0|
// +--------------+


```



Untyped User-Defined Aggregate Functions
Typed aggregations, as described above, may also be registered as untyped aggregating UDFs for use with DataFrames. For example, a user-defined average for untyped DataFrames can look like:

```sql
-- Compile and place UDAF MyAverage in a JAR file called `MyAverage.jar` in /tmp.
CREATE FUNCTION myAverage AS 'MyAverage' USING JAR '/tmp/MyAverage.jar';

SHOW USER FUNCTIONS;
+------------------+
|          function|
+------------------+
| default.myAverage|
+------------------+

CREATE TEMPORARY VIEW employees
USING org.apache.spark.sql.json
OPTIONS (
    path "examples/src/main/resources/employees.json"
);

SELECT * FROM employees;
+-------+------+
|   name|salary|
+-------+------+
|Michael|  3000|
|   Andy|  4500|
| Justin|  3500|
|  Berta|  4000|
+-------+------+

SELECT myAverage(salary) as average_salary FROM employees;
+--------------+
|average_salary|
+--------------+
|        3750.0|
+--------------+
```





## Integration with Hive UDFs/UDAFs/UDTFs

https://spark.apache.org/docs/latest/sql-ref-functions-udf-hive.html


### Description
Spark SQL supports integration of Hive UDFs, UDAFs and UDTFs. Similar to Spark UDFs and UDAFs, Hive UDFs work on a single row as input and generate a single row as output, while Hive UDAFs operate on multiple rows and return a single aggregated row as a result. In addition, Hive also supports UDTFs (User Defined Tabular Functions) that act on one row as input and return multiple rows as output. To use Hive UDFs/UDAFs/UTFs, the user should register them in Spark, and then use them in Spark SQL queries.

### Examples
Hive has two UDF interfaces: UDF and GenericUDF. An example below uses GenericUDFAbs derived from GenericUDF.
```sql
-- Register `GenericUDFAbs` and use it in Spark SQL.
-- Note that, if you use your own programmed one, you need to add a JAR containing it
-- into a classpath,
-- e.g., ADD JAR yourHiveUDF.jar;
CREATE TEMPORARY FUNCTION testUDF AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDFAbs';

SELECT * FROM t;
+-----+
|value|
+-----+
| -1.0|
|  2.0|
| -3.0|
+-----+

SELECT testUDF(value) FROM t;
+--------------+
|testUDF(value)|
+--------------+
|           1.0|
|           2.0|
|           3.0|
+--------------+
```
An example below uses GenericUDTFExplode derived from GenericUDTF.
```sql
-- Register `GenericUDTFExplode` and use it in Spark SQL
CREATE TEMPORARY FUNCTION hiveUDTF
AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDTFExplode';

SELECT * FROM t;
+------+
| value|
+------+
|[1, 2]|
|[3, 4]|
+------+

SELECT hiveUDTF(value) FROM t;
+---+
|col|
+---+
|  1|
|  2|
|  3|
|  4|
+---+
Hive has two UDAF interfaces: UDAF and GenericUDAFResolver. An example below uses GenericUDAFSum derived from GenericUDAFResolver.

-- Register `GenericUDAFSum` and use it in Spark SQL
CREATE TEMPORARY FUNCTION hiveUDAF
AS 'org.apache.hadoop.hive.ql.udf.generic.GenericUDAFSum';

SELECT * FROM t;
+---+-----+
|key|value|
+---+-----+
|  a|    1|
|  a|    2|
|  b|    3|
+---+-----+

SELECT key, hiveUDAF(value) FROM t GROUP BY key;
+---+---------------+
|key|hiveUDAF(value)|
+---+---------------+
|  b|              3|
|  a|              3|
+---+---------------+
```
