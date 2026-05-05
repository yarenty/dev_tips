## Quite simple example

https://github.com/luojia65/plugin-system-example


## Inventory

CHECK THIS!!!



This crate provides a way to set up a plugin registry into which plugins can be registered from any source file linked into your application. There does not need to be a central list of all the plugins.



https://github.com/dtolnay/inventory




## Plugins system in Rust

https://nullderef.com/series/rust-plugins/

A Plugin System in Rust
My LFX mentorship with Tremor, in which I implement a Plugin System to maximize modularity and reduce Rust’s wild compilation times. This is done with dynamic loading, but its alternatives are also covered in detail in the first articles.
Plugins in Rust: The Technologies
1. Plugins in Rust: The Technologies
   Welcome to the “Plugins in Rust” series! During the next months I’ll be involved in a project with Tremor, for which I need to implement a Plugin System. The goal is to maximize modularity and reduce Rust’s wild compilation times. The implementation will end up being done with dynamic loading, but I will cover all the alternatives first to make sure it’s the best approach for my use-case. In this first article I will analyze the different ways in which our plugin system may be implemented in Rust....

May 17, 2021 · 22 min · Mario Ortiz Manero
Plugins in Rust: Getting Started
2. Plugins in Rust: Getting Started
   Welcome to the second article of my “Plugins in Rust” series! Here I will try to actually write some simple code of what a plugin system might look like, and try to answer any questions that may arise while doing so. Even though the project is specific to Tremor, I try to keep it relatively generic so that it’s useful to future rustaceans interested in plugin systems. So if you don’t really care about Tremor’s specific case, you can skip to the next section....

September 5, 2021 · 20 min · Mario Ortiz Manero
Plugins in Rust: Diving into Dynamic Loading
3. Plugins in Rust: Diving into Dynamic Loading
   In the last article of this series I wrote some simple experiments of plugins both with WebAssembly and dynamic loading. After discarding Wasm for this specific plugin system, I wanted to try to get a more realistic example with dynamic loading and work towards the final implementation. Since the Tremor team suggested that I began by implementing connectors, we’ll first have to learn more about them. Matthias ran me through their current state in a meeting, which I’ll try to summarize....

October 5, 2021 · 27 min · Mario Ortiz Manero
Plugins in Rust: Reducing the Pain with Dependencies
4. Plugins in Rust: Reducing the Pain with Dependencies
   Previously in this series, I covered how the plugin system could be implemented from scratch. This is a lot of work if you’re dealing with a relatively large codebase and therefore a complex interface in your plugin system, so let’s see how we can make our lives easier. I’ve been wanting to try abi_stable for this since the beginning, which was specifically created for plugins. But we aren’t really locked to that crate, so I’ll show other alternatives as well, which can even be combined to your liking....

November 8, 2021 · 28 min · Mario Ortiz Manero
Plugins in Rust: Getting our Hands Dirty
5. Plugins in Rust: Getting our Hands Dirty
   Welcome to one of the last articles of this series! Previously, we covered how to use external dependencies to lessen the work necessary to implement our plugin system. Now that we know how to actually get started, we’ll implement it once and for all. I will personally use the crate abi_stable , but the concepts should be roughly the same for any dynamic loading method. Similarly, some of my advice is related to modifying an already existing project of large size....

February 11, 2022 · 28 min · Mario Ortiz Manero
Plugins in Rust: Wrapping Up
6. Plugins in Rust: Wrapping Up
   Welcome to the final article of this series! Here I’ll showcase some clean-ups and optimizations I may or may not have performed yet, so that our plugin system can get closer to production. I will also run benchmarks for some of these to ensure that the changes are actually worth it. 1. Benchmarking 1.1. Tooling There are all kinds of ways to measure the performance of a system, and it’s important to take as many of them as possible into account....

July 26, 2022 · 26 min · Mario Ortiz Manero
[Talk] Rust, the best and worst thing to happen to Tremor
7. [Talk] Rust, the best and worst thing to happen to Tremor
   Hello! I’ve recently had the pleasure of giving a talk at this year’s TremorCon. I really enjoy public speaking, so this has been an excellent opportunity to get better at it (let me know your thoughts!). In case you missed it, you can check out the recording here: You can also find the slides here in pdf or in pptx. If you want to learn about new events I’m involved in, like this one, you can follow me on Twitter or LinkedIn....

October 18, 2022 · 1 min · Mario Ortiz Manero

