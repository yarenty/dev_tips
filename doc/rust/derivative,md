
# Derivative

https://mcarton.github.io/rust-derivative/latest/index.html


derivative = "2.2.0"



## Debuging

There is standard debuging, but here you can creae deuging even when fileds do not implement debug itself:
 - ignoring them
 - adding extra formatter // to be tested 



```rust
use derivative::Derivative;


#[derive(Derivative)]
#[derivative(Debug)]
struct SQLTableSource {
    #[derivative(Debug="ignore")]
    provider: Arc<SQLFederationProvider>,
    table_name: String,
    schema: SchemaRef,
}

// TODO: test this one
// #[derivative(Debug(format_with="path::to::my_fmt_fn"))]
//fn fmt(&T, &mut std::fmt::Formatter) -> Result<(), std::fmt::Error>;

```

## Default

```rust

#![allow(unused)]
fn main() {
extern crate derivative;
use derivative::Derivative;
#[derive(Debug, Derivative)]
#[derivative(Default(new="true"))]
struct Foo {
    foo: u8,
    bar: u8,
}

println!("{:?}", Foo::new()); // Foo { foo: 0, bar: 0 }
}


```

## Hash & Default