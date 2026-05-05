# HalfBrown

https://crates.io/crates/halfbrown


Hashmap implementation that dynamically switches from a vector based backend to a hashbrown based backend as the number of keys grows

Note: The heavy lifting in this is done in hashbrown, and the docs and API are copied from them.

Halfbrown, is a hashmap implementation that uses two backends to optimize for different cernairos:


# hashbrown
 now part of official rust hashmap


# pin-project

A crate for safe and ergonomic pin-projection.

https://crates.io/crates/pin-project



# tinyvec

https://crates.io/crates/tinyvec


A 100% safe crate of vec-like types. #![forbid(unsafe_code)]

Main types are as follows:

ArrayVec is an array-backed vec-like data structure. It panics on overflow.
SliceVec is the same deal, but using a &mut [T].
TinyVec (alloc feature) is an enum that's either an Inline(ArrayVec) or a Heap(Vec). If a TinyVec is Inline and would overflow it automatically transitions to Heap and continues whatever it was doing.
To attain this "100% safe code" status there is one compromise: the element type of the vecs must implement Default.
