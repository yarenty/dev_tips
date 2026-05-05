# Snowflake
https://docs.snowflake.com/en/developer-guide/udf/sql/udf-sql.html


https://docs.snowflake.com/en/sql-reference/udf-overview.html


Main output:
- Snowflake supports scalar and tabular UDFS - no UDAF!
- There are multiple languages supported:
    - SQL
    - JavaScript
    - Java
    - Python
- Only Java is precompiled 
- Python is staged
- SQL is processed on-demand


## Supported languages

| Language | Tabular | Looping and Branching | Pre-Compiled or Staged Source | In-line | Sharable |
|---|---|---------------------|-----------------------------|---------|----------|
|SQL| Yes| No                  | No                          | Yes     | Yes      |
|JavaScript|Yes| Yes                 | No                          | Yes     |Yes
|Java|Yes|Yes|Yes (precompiled)|Yes|No [1](1)
|Python|Yes|Yes|Yes (staged)|Yes|No [2](2)




[1]: Java UDFs are not sharable. Database objects that use Java UDFs are also not sharable. For example, you cannot:

- Directly share a Java UDF.
- Share a view that calls a Java UDF.
- Share a function that calls a Java UDF.
- Share a table with a masking or row access policy that calls a Java UDF.


[2]: Python UDFs and modules brought in through stages must be platform-independent and must not contain native extensions.
- Avoid code that assumes a specific CPU architecture (e.g. x86).
- Avoid code that assumes a specific operating system.
Python UDFs are not sharable. Database objects that use Python UDFs are also not sharable. For example, you cannot:
- Directly share a Python UDF.
- Share a view that calls a Python UDF.
- Share a function that calls a Python UDF.
- Share a table with a masking or row access policy that calls a Python UDF.


## Examples

```sql
CREATE FUNCTION add_pi(PARAM_1 FLOAT)
    RETURNS FLOAT
    LANGUAGE SQL
    AS $$
        PARAM_1 + 3.1415926::FLOAT
    $$;
```



## SQL UDF

A SQL UDF can be either scalar or tabular.

A SQL UDF evaluates an arbitrary SQL expression and returns the result(s) of the expression.

The function definition can be a SQL expression that returns either a scalar (i.e. single) value or, if defined as a table function, a set of rows.

This UDF uses the general expression pi() * radius * radius:
```sql 
CREATE FUNCTION area_of_circle(radius FLOAT)
RETURNS FLOAT
AS
$$
pi() * radius * radius
$$
;
This UDF uses a query expression:
```

```sql
CREATE FUNCTION profit()
RETURNS NUMERIC(11, 2)
AS
$$
SELECT SUM((retail_price - wholesale_price) * number_sold)
FROM purchases
$$
;
```

For non-tabular UDFs, the query expression must be guaranteed to return at most one row, containing one column.

The expression defining a UDF can refer to the input arguments of the function, as shown in the examples above.

The expression defining a UDF can refer to database objects such as tables, views, and sequences, as shown in the examples above. However, the following restrictions apply:

The UDF owner must have appropriate privileges on any database objects that the UDF accesses.

Database objects cannot be referred to using dynamic SQL. For example, the following statement fails because IDENTIFIER(table_name_parameter) is not allowed:

```sql
CREATE OR REPLACE FUNCTION profit2(table_name_parameter VARCHAR)
RETURNS NUMERIC(11, 2)
AS
$$
SELECT SUM((retail_price - wholesale_price) * number_sold)
FROM IDENTIFIER(table_name_parameter)
$$
;
```
The expression defining a UDF should not contain a semicolon as a statement terminator. For example, note that the SELECT in the query expression UDF above is not terminated with a semicolon.

A SQL UDF’s defining expression can refer to other user-defined functions. However, it cannot refer recursively to itself, either directly or indirectly (i.e. through another function calling back to it).

>Note:  
>Although the body of a UDF can contain a complete SELECT statement, it cannot contain DDL statements or any DML statement other than SELECT.


## WINDOW functions

https://docs.snowflake.com/en/sql-reference/functions-analytic.html




## Examples

### Basic SQL Scalar UDF Example(s)
This example returns a hard-coded approximation of the mathematical constant pi.
```sql
create function pi_udf()
returns float
as '3.141592654::FLOAT'
;

select pi_udf();   
```
Output:
```log
select pi_udf();
+-------------+
|    PI_UDF() |
|-------------|
| 3.141592654 |
+-------------+
```

### Common SQL Examples
Query Expression with SELECT Statement
Create the table and data to use:
```sql
create table purchases (number_sold integer, wholesale_price number(7,2), retail_price number(7,2));
insert into purchases (number_sold, wholesale_price, retail_price) values
(3,  10.00,  20.00),
(5, 100.00, 200.00)
;
```
Create the UDF:
```sql
create function profit()
returns numeric(11, 2)
as
$$
select sum((retail_price - wholesale_price) * number_sold)
from purchases
$$
;
```
Call the UDF in a query:
```log
select profit();
Output:

select profit();
+----------+
| PROFIT() |
|----------|
|   530.00 |
+----------+
```

### UDF in a WITH Clause
```sql
create table circles (diameter float);

insert into circles (diameter) values
(2.0),
(4.0);
```
```sql
create function diameter_to_radius(f float)
returns float
as
$$ f / 2 $$
;
with
radii as (select diameter_to_radius(diameter) as radius from circles)
select radius from radii
order by radius
;
```
Output:
```log
+--------+
| RADIUS |
|--------|
|      1 |
|      2 |
+--------+
```

### JOIN Operation
This example uses a more complex query, which includes a JOIN operation:

Create the table and data to use:
```sql
create table orders (product_id varchar, quantity integer, price numeric(11, 2), buyer_info varchar);
create table inventory (product_id varchar, quantity integer, price numeric(11, 2), vendor_info varchar);
insert into inventory (product_id, quantity, price, vendor_info) values
('X24 Bicycle', 4, 1000.00, 'HelloVelo'),
('GreenStar Helmet', 8, 50.00, 'MellowVelo'),
('SoundFX', 5, 20.00, 'Annoying FX Corporation');
insert into orders (product_id, quantity, price, buyer_info) values
('X24 Bicycle', 1, 1500.00, 'Jennifer Juniper'),
('GreenStar Helmet', 1, 75.00, 'Donovan Liege'),
('GreenStar Helmet', 1, 75.00, 'Montgomery Python');
```
Create the UDF:
```sql
create function store_profit()
returns numeric(11, 2)
as
$$
select sum( (o.price - i.price) * o.quantity)
from orders as o, inventory as i
where o.product_id = i.product_id
$$
;
```
Call the UDF in a query:
```sql
select store_profit();
```
Output:
```log
select store_profit();
+----------------+
| STORE_PROFIT() |
|----------------|
|         550.00 |
+----------------+
```

### Using UDFs in Different Clauses
A scalar UDF can be used any place a scalar expression can be used. For example:
```sql
-- ----- These examples show a UDF called from different clauses ----- --

select myfunc(column1) from table1;

select * from table1 where column2 > myfunc(column1);
```

### Using SQL Variables in a UDF
This example shows how to set a SQL variable and use that variable inside a UDF:
```sql
set id_threshold = (select count(*)/2 from table1);
create or replace function my_filter_function()
returns table (id int)
as
$$
select id from table1 where id > $id_threshold
$$
;
```


## Tabular UDF

https://docs.snowflake.com/en/developer-guide/udf/sql/udf-sql-tabular-functions.html


```sql

CREATE OR REPLACE FUNCTION <name> ( [ <arguments> ] )
  RETURNS TABLE ( <output_col_name> <output_col_type> [, <output_col_name> <output_col_type> ... ] )
  AS '<sql_expression>'
  

```


```sql
create function t()
    returns table(msg varchar)
    as
    $$
        select 'Hello'
        union
        select 'World'
    $$;
    
```