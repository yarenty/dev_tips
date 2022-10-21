# bytemuck

A crate for mucking around with piles of bytes.

This crate lets you safely perform "bit cast" operations between data types. That's where you take a value and just reinterpret the bits as being some other type of value, without changing the bits.

This is not like the as keyword
This is not like the From trait
It is most like f32::to_bits, just generalized to let you convert between all sorts of data types.


https://crates.io/crates/bytemuck

## bitfrob

https://crates.io/crates/bitfrob
