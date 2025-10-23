# MariaDB
https://github.com/pluots/sql-udf

https://github.com/pluots/sql-udf/tree/main/udf-examples


You will need to enter a SQL console to load the functions. This can be done with the mariadb/mysql command, either on your host system or within the docker container. If you used the provided Dockerfile.examples, the password is example.

docker exec -it mariadb_udf_test mysql -pexample
Once logged in, you can load all available example functions:
```sql
CREATE FUNCTION is_const RETURNS string SONAME 'libudf_examples.so';
CREATE FUNCTION lookup6 RETURNS string SONAME 'libudf_examples.so';
CREATE FUNCTION sum_int RETURNS integer SONAME 'libudf_examples.so';
CREATE FUNCTION udf_sequence RETURNS integer SONAME 'libudf_examples.so';
CREATE FUNCTION lipsum RETURNS string SONAME 'libudf_examples.so';
CREATE FUNCTION log_calls RETURNS integer SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION avg2 RETURNS real SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION avg_cost RETURNS real SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION udf_median RETURNS integer SONAME 'libudf_examples.so';
```


