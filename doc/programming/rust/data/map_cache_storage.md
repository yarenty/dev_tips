# DashMap

https://crates.io/crates/dashmap

Blazingly fast concurrent map in Rust.

DashMap is an implementation of a concurrent associative array/hashmap in Rust.

DashMap tries to implement an easy to use API similar to std::collections::HashMap with some slight changes to handle concurrency.

DashMap tries to be very simple to use and to be a direct replacement for RwLock<HashMap<K, V>>. To accomplish these goals, all methods take &self instead of modifying methods taking &mut self. This allows you to put a DashMap in an Arc<T> and share it between threads while still being able to modify it.

DashMap puts great effort into performance and aims to be as fast as possible. If you have any suggestions or tips do not hesitate to open an issue or a PR.



# Memory storage

## EVMap

A lock-free, eventually consistent, concurrent multi-value map.

This map implementation allows reads and writes to execute entirely in parallel, with no implicit synchronization overhead. Reads never take locks on their critical path, and neither do writes assuming there is a single writer (multi-writer is possible using a Mutex), which significantly improves performance under contention. It is backed by the concurrency primitive left-right.

The trade-off exposed by this module is one of eventual consistency: writes are not visible to readers except following explicit synchronization. Specifically, readers only see the operations that preceeded the last call to WriteHandle::flush by a writer. This lets writers decide how stale they are willing to let reads get. They can refresh the map after every write to emulate a regular concurrent HashMap, or they can refresh only occasionally to reduce the synchronization overhead at the cost of stale reads.

For read-heavy workloads, the scheme used by this module is particularly useful. Writers can afford to refresh after every write, which provides up-to-date reads, and readers remain fast as they do not need to ever take locks.

The map is multi-value, meaning that every key maps to a collection of values. This introduces some memory cost by adding a layer of indirection through a Vec for each value, but enables more advanced use. This choice was made as it would not be possible to emulate such functionality on top of the semantics of this map (think about it -- what would the operational log contain?).

To facilitate more advanced use-cases, each of the two maps also carry some customizeable meta-information. The writers may update this at will, and when a refresh happens, the current meta will also be made visible to readers. This could be useful, for example, to indicate what time the refresh happened.


https://crates.io/crates/evmap


https://github.com/jonhoo/evmap



# Cache

## Moka

https://crates.io/crates/moka


Moka is a fast, concurrent cache library for Rust. Moka is inspired by the Caffeine library for Java.

Moka provides cache implementations on top of hash maps. They support full concurrency of retrievals and a high expected concurrency for updates. Moka also provides a non-thread-safe cache implementation for single thread applications.

All caches perform a best-effort bounding of a hash map using an entry replacement algorithm to determine which entries to evict when the capacity is exceeded.





## quick cache

https://crates.io/crates/quick_cache


Lightweight and high performance concurrent cache optimized for low cache overhead.

- Small overhead compared to a concurrent hash table
- Scan resistent and high hit rate caching policy
- Scales well with the number of threads
- Doesn't use background threads
- 100% safe code in the crate
- Small dependency tree

- The implementation is optimized for use cases where the cache access times and overhead can add up to be a significant cost. Features like: time to live, cost weighting, or event listeners; are not current implemented in Quick Cache. If you need these features you may want to take a look at the Moka crate.


