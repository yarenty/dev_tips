
# SQLIte loadable



https://github.com/asg017/sqlite-loadable-rs

https://crates.io/crates/sqlite-loadable




A framework for building loadable SQLite extensions in Rust. Inspired by rusqlite, pgx, and Riyaz Ali's similar SQLite Go library.



## Background
SQLite's runtime loadable extensions allows one to add new scalar functions, table functions, virtual tables, virtual filesystems, and more to a SQLite database connection. These compiled dynamically-linked libraries can be loaded in any SQLite context, including the SQLite CLI, Python, Node.js, Rust, Go, and many other languages.

Note Notice the word loadable. Loadable extensions are these compiled dynamically-linked libraries, with a suffix of .dylib or .so or .dll (depending on your operating system). These are different than application-defined functions that many language clients support (such as Python's .create_function() or Node.js's .function()).

Historically, the main way one could create these loadable SQLite extensions were with C/C++, such as spatilite, the wonderful sqlean project, or SQLite's official miscellaneous extensions.

But C is difficult to use safely, and integrating 3rd party libraries can be a nightmare. Riyaz Ali wrote a Go library that allows one to easily write loadable extensions in Go, but it comes with a large performance cost and binary size. For Rust, rusqlite has had a few different PRs that attempted to add loadable extension support in that library, but none have been merged.

So, sqlite-loadable-rs is the first and most involved framework for writing loadable SQLite extensions in Rust!

## Features

### Scalar functions

Scalar functions are the simplest functions one can add to SQLite - take in values as inputs, and return a value as output. To implement one in sqlite-loadable-rs, you just need to call define_scalar_function on a "callback" Rust function decorated with #[sqlite_entrypoint], and you'll be able to call it from SQL!
```rust
// add(a, b)
fn add(context: *mut sqlite3_context, values: &[*mut sqlite3_value]) -> Result<()> {
    let a = api::value_int(values.get(0).expect("1st argument"));
    let b = api::value_int(values.get(1).expect("2nd argument"));
    api::result_int(context, a + b);
    Ok(())
}

// connect(seperator, string1, string2, ...)
fn connect(context: *mut sqlite3_context, values: &[*mut sqlite3_value]) -> Result<()> {
    let seperator = api::value_text(values.get(0).expect("1st argument"))?;
    let strings:Vec<&str> = values
        .get(1..)
        .expect("more than 1 argument to be given")
        .iter()
        .filter_map(|v| api::value_text(v).ok())
        .collect();
    api::result_text(context, &strings.join(seperator))?;
    Ok(())
}
#[sqlite_entrypoint]
pub fn sqlite3_extension_init(db: *mut sqlite3) -> Result<()> {
    define_scalar_function(db, "add", 2, add, FunctionFlags::DETERMINISTIC)?;
    define_scalar_function(db, "connect", -1, connect, FunctionFlags::DETERMINISTIC)?;
    Ok(())
}
```
```sql
sqlite> select add(1, 2);
3
sqlite> select connect('-', 'alex', 'brian', 'craig');
alex-brian-craig
```

### Table functions

Table functions, (aka "Eponymous-only virtual tables"), can be added to your extension with define_table_function.

define_table_function::<CharactersTable>(db, "characters", None)?;

Defining a table function is complicated and requires a lot of code - see the characters.rs example for a full solution.

Once compiled, you can invoke a table function like querying any other table, with any arguments that the table function supports.
```sql
sqlite> .load target/debug/examples/libcharacters
sqlite> select rowid, * from characters('alex garcia');
┌───────┬───────┐
│ rowid │ value │
├───────┼───────┤
│ 0     │ a     │
│ 1     │ l     │
│ 2     │ e     │
│ 3     │ x     │
│ 4     │       │
│ 5     │ g     │
│ 6     │ a     │
│ 7     │ r     │
│ 8     │ c     │
│ 9     │ i     │
│ 10    │ a     │
└───────┴───────┘
```
Some real-world non-Rust examples of table functions in SQLite:

- json_each / json_tree
- generate_series
- pragma_* functions
- html_each


### Virtual tables

sqlite-loadable-rs also supports more traditional virtual tables, for tables that have a dynamic schema or need insert/update support.

define_virtual_table() can define a new read-only virtual table module for the given SQLite connection. define_virtual_table_writeable() is also available for tables that support INSERT/UPDATE/DELETE, but this API will probably change.

define_virtual_table::<CustomVtab>(db, "custom_vtab", None)?
These virtual tables can be created in SQL with the CREATE VIRTUAL TABLE syntax.


create virtual table xxx using custom_vtab(arg1=...);

select * from xxx;

Some real-world non-Rust examples of traditional virtual tables in SQLite include the CSV virtual table, the full-text search fts5 extension, and the R-Tree extension.

### Examples
The examples/ directory has a few bare-bones examples of extensions, which you can build with:

```shell
$ cargo build --example hello
$ sqlite3 :memory: '.load target/debug/examples/hello' 'select hello("world");'
hello, world!

# Build all the examples in release mode, with output at target/debug/release/examples/*.dylib
$ cargo build --example --release
```
Some real-world projects that use sqlite-loadable-rs:

- sqlite-xsv - An extremely fast CSV/TSV parser in SQLite
- sqlite-regex - An extremely fast and safe regular expression library for SQLite
- sqlite-base64 - Fast base64 encoding and decoding in SQLite

