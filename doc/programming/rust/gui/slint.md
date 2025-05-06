# Slint


https://slint.rs/


Declarative GUI for Rust


```rust
slint::slint!{
    export component HelloWorld {
        Text {
            text: "hello world";
            color: green;
        }
    }
}
fn main() {
    HelloWorld::new().unwrap().run().unwrap();
}

```


```toml
[package]
...
build = "build.rs"
edition = "2021"

[dependencies]
slint = "1.5.0"
...

[build-dependencies]
slint-build = "1.5.0"

```


Use the API of the slint-build crate in the build.rs file:

fn main() {
slint_build::compile("ui/hello.slint").unwrap();
}
Finally, use the include_modules! macro in your main.rs:

ⓘ
slint::include_modules!();
fn main() {
HelloWorld::new().unwrap().run().unwrap();
}
The cargo-generate tool is a great tool to up and running quickly with a new Rust project. You can use it in combination with our Template Repository to create a skeleton file hierarchy that uses this method:

cargo install cargo-generate
cargo generate --git https://github.com/slint-ui/slint-rust-template



## Slint

https://slint.dev/releases/1.5.1/docs/rust/slint/


https://slint.dev/releases/1.5.1/editor/



# Tao


Crate taoCopy item path
source · [−]
Tao is a cross-platform application window creation and event loop management library.


https://docs.rs/tao/latest/tao/







## Chkawka


see krokiet!!!


https://github.com/qarmin/czkawka

