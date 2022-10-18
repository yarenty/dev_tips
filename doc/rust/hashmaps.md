# HalfBrown

https://crates.io/crates/halfbrown


Hashmap implementation that dynamically switches from a vector based backend to a hashbrown based backend as the number of keys grows

Note: The heavy lifting in this is done in hashbrown, and the docs and API are copied from them.

Halfbrown, is a hashmap implementation that uses two backends to optimize for different cernairos:

