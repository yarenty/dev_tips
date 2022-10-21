# RustCrypto: POLYVAL


https://crates.io/crates/polyval


POLYVAL (RFC 8452) is a universal hash function which operates over GF(2^128) and can be used for constructing a Message Authentication Code (MAC).

Its primary intended use is for implementing AES-GCM-SIV, however it is closely related to GHASH and therefore can also be used to implement AES-GCM at no cost to performance on little endian architectures.