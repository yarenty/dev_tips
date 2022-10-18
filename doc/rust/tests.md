# mockall

https://crates.io/crates/mockall

Mock objects are a powerful technique for unit testing software. A mock object is an object with the same interface as a real object, but whose responses are all manually controlled by test code. They can be used to test the upper and middle layers of an application without instantiating the lower ones, or to inject edge and error cases that would be difficult or impossible to create when using the full stack.

Statically typed languages are inherently more difficult to mock than dynamically typed languages. Since Rust is a statically typed language, previous attempts at creating a mock object library have had mixed results. Mockall incorporates the best elements of previous designs, resulting in it having a rich feature set with a terse and ergonomic interface. Mockall is written in 100% safe and stable Rust.


# tux

https://crates.io/crates/tux


Miscellaneous test utilities for unit and integration tests in Rust.

[dev-dependencies]
tux = { version = "0.2" }
The goal of this project is to support a variety of test scenarios that may be tricky to test, such as HTTP requests, executing binaries, testing complex output, etc.

There is no particular scope with the code utilities provided, other than being generally useful in a unit or integration test scenario.


HAS!!! temp dirs / files  and HTTP requests

