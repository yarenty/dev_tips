# sql-parse

https://crates.io/crates/sql-parse

**Parse SQL into an AST**

This crate provides an lexer and parser that can parse SQL into an Abstract Syntax Tree (AST). Currently primarily focused on MariaDB/Mysql.

Example code:
```rust
use sql_parse::{SQLDialect, SQLArguments, ParseOptions, parse_statement};

let options = ParseOptions::new()
.dialect(SQLDialect::MariaDB)
.arguments(SQLArguments::QuestionMark)
.warn_unquoted_identifiers(true);

let mut issues = Vec::new();

let sql = "SELECT `monkey`,
FROM `t1` LEFT JOIN `t2` ON `t2`.`id` = `t1.two`
WHERE `t1`.`id` = ?";

let ast = parse_statement(sql, &mut issues, &options);

println!("Issues: {:#?}", issues);
println!("AST: {:#?}", ast);
```


## Features

* Good error recovery: The parser implements reasonable error recovery and will continue parsing long expressions if an error is found within.
* Code span annotations: All AST notes implements Spanned that yields a byte span within the code. This means that errors and warnings generated from the parsing can be precented to the user in a nice ways. Also users of the AST can generate more issues that can also similarly be presented nicely.
* No dependencies: We use no-std with alloc, and has no other dependencies
* No unsafe code: We use #![forbid(unsafe_code)] to guarantee no unsafe code.
* Fast parsing: The parser is a hand written recursive decent parser. To speed up parser expressions are parsed using a O(1) shift reduce mechanism.


## Comments 
- looks like not touched for over year (17 Jun 2021)

## TODO

- How to create own / Datafusion dialect - currently MariaDB, PostgreSQL
