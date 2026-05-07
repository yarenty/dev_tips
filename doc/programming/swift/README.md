---
title: "Swift"
keywords: [swift, ios, macos, vapor, hummingbird, swift-package-manager, foundation]
status: reviewed
review_date: 2026/05/03
---

# Swift

Sparse landing — no in-depth Swift articles here yet. Swift is a much wider language than its "Apple platforms only" stereotype suggests, but most of what's written up in this vault is on the Apple-platform side under [[ios]].

## Where Swift runs in 2024-25

- **Apple platforms** (iOS, macOS, watchOS, tvOS, visionOS) — the original target. Develop in Xcode; UI in SwiftUI (modern) or UIKit/AppKit (legacy-but-still-required). See [[ios]].
- **Linux** (and increasingly Windows) — server-side Swift is real. The two main web frameworks are [Vapor](https://vapor.codes/) (older, Express-like) and [Hummingbird](https://hummingbird.codes/) (newer, lighter). Apple itself uses Swift on the server.
- **Embedded** — [Embedded Swift](https://www.swift.org/blog/embedded-swift-examples/) is a recent subset of the language designed for microcontrollers (no runtime allocation, no metadata). Targets ARM Cortex-M, RISC-V, ESP32.

## What's interesting about modern Swift

- **Concurrency** (`async`/`await`, structured concurrency, actors, `Sendable`) — one of the more thoughtful concurrency models in mainstream languages. Borrows ideas from Rust's `Send`/`Sync` but with a different ergonomic story.
- **Foundation rewrite** — Apple is rewriting Foundation in pure Swift (separating it from Objective-C heritage). Means better cross-platform Swift on Linux/Windows, and clearer semantics generally.
- **C++ interop** — Swift now has direct C++ interop, not just C. That's a significant move; combined with its existing Objective-C bridging, Swift can reach into a lot of legacy native code.
- **Swift Package Manager (SPM)** — the modern way to depend on Swift code. `Package.swift` + `Package.resolved` lockfile. CocoaPods and Carthage are legacy on iOS too now.
- **swift-syntax / macros** — Swift 5.9+ added macros built on the same syntax tree library Apple uses internally. Closer to Rust proc-macros than C-style preprocessor macros.

## Canonical external resources

- [swift.org](https://www.swift.org/) — the official site post the move out of Apple's exclusive control.
- [The Swift Programming Language (book)](https://docs.swift.org/swift-book/) — official, free, very good.
- [Swift Forums](https://forums.swift.org/) — primary venue for language evolution discussion.
- [Hacking with Swift](https://www.hackingwithswift.com/) — Paul Hudson's site; the most-recommended task-oriented learning material.
- [Swift Server Workgroup](https://www.swift.org/sswg/) — server-side ecosystem coordination.

## See also

- [[ios]] — App-Store / iOS-specific notes.
- [[programming/package_managers/README|package managers]] — Swift Package Manager context.
- [[programming/rust/README|Rust]] — comparable systems-with-safety language; very different ergonomic choices.
- [[programming/README|programming]] — parent.

## Keywords

`#swift` `#ios` `#macos` `#vapor` `#hummingbird` `#swift-package-manager` `#programming`
