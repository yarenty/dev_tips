# Debug

## default

```rust
#[derive(Debug)]
pub struct PubStruct {
    a: bool,
    b: usize,
}
```

## debug_stub_derive

https://crates.io/crates/debug_stub_derive


https://docs.rs/debug_stub_derive/0.3.0/debug_stub_derive/


```rust
#[macro_use]
extern crate debug_stub_derive;

// A struct from an external crate which does not implement the `fmt::Debug`
// trait.
pub struct ExternalCrateStruct;

// A struct in the current crate which wants to cleanly expose
// itself to the outside world with an implementation of `fmt::Debug`.
#[derive(DebugStub)]
pub struct PubStruct {
    a: bool,
    // Define a replacement debug serialization for the external struct.
    #[debug_stub="ReplacementValue"]
    b: ExternalCrateStruct
}

assert_eq!(format!("{:?}", PubStruct {
    a: true,
    b: ExternalCrateStruct

}), "PubStruct { a: true, b: ReplacementValue }");
```

## derivative

I think most powerful - 
see [derivative](derivative.md)
