# YEW

https://yew.rs/

https://github.com/yewstack/yew



Yew is a modern Rust framework for creating multi-threaded front-end web apps using WebAssembly.

It features a component-based framework which makes it easy to create interactive UIs. Developers who have experience with frameworks like React and Elm should feel quite at home when using Yew.
It achieves great performance by minimizing DOM API calls and by helping developers to easily offload processing to background threads using web workers.
It supports JavaScript interoperability, allowing developers to leverage NPM packages and integrate with existing JavaScript applications.



# Gloo
A toolkit for building fast, reliable Web applications and libraries with Rust and Wasm.


Gloo is a collection of libraries, and those libraries provide ergonomic Rust wrappers for browser APIs. web-sys/js-sys are very difficult/inconvenient to use directly so provides wrappers around the raw bindngs which makes it easier to consume those APIs. This is why it is called a "toolkit", instead of "library" or "framework".