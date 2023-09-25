# tokio


# rayon
https://github.com/rayon-rs/rayon


Rayon is a data-parallelism library for Rust. It is extremely lightweight and makes it easy to convert a sequential computation into a parallel one. It also guarantees data-race freedom. (You may also enjoy this blog post about Rayon, which gives more background and details about how it works, or this video, from the Rust Belt Rust conference.) Rayon is available on crates.io, and API Documentation is available on docs.rs.





# Flume

A blazingly fast multi-producer, multi-consumer channel.


```rust
use std::thread;

fn main() {
println!("Hello, world!");

    let (tx, rx) = flume::unbounded();

    thread::spawn(move || {
        (0..10).for_each(|i| {
            tx.send(i).unwrap();
        })
    });

    let received: u32 = rx.iter().sum();

    assert_eq!((0..10).sum::<u32>(), received);
}
```


**Why Flume?**

- Featureful: Unbounded, bounded and rendezvous queues
- Fast: Always faster than std::sync::mpsc and sometimes crossbeam-channel
- Safe: No unsafe code anywhere in the codebase!
- Flexible: Sender and Receiver both implement Send + Sync + Clone
- Familiar: **Drop-in replacement for std::sync::mpsc**
- Capable: Additional features like MPMC support and send timeouts/deadlines
- Simple: Few dependencies, minimal codebase, fast to compile
- Asynchronous: async support, including mix 'n match with sync code
- Ergonomic: Powerful select-like interface



# Flurry

A concurrent hash table based on Java’s ConcurrentHashMap.


https://docs.rs/flurry/latest/flurry/

# DashMap


https://docs.rs/dashmap/latest/dashmap/struct.DashMap.html

DashMap tries to be very simple to use and to be a direct replacement for RwLock<HashMap<K, V, S>>. To accomplish this, all methods take &self instead of modifying methods taking &mut self. This allows you to put a DashMap in an Arc<T> and share it between threads while being able to modify it.





# Crossbeam 

https://github.com/crossbeam-rs/crossbeam

This crate provides a set of tools for concurrent programming:

- Atomics
  - AtomicCell, a thread-safe mutable memory location.(no_std)
  - AtomicConsume, for reading from primitive atomic types with "consume" ordering.(no_std)

- Data structures
  - deque, work-stealing deques for building task schedulers.
  - ArrayQueue, a bounded MPMC queue that allocates a fixed-capacity buffer on construction.(alloc)  
  - SegQueue, an unbounded MPMC queue that allocates small buffers, segments, on demand.(alloc)

- Memory management
  - epoch, an epoch-based garbage collector.(alloc)

- Thread synchronization 
  - channel, multi-producer multi-consumer channels for message passing.
  - Parker, a thread parking primitive.
  - ShardedLock, a sharded reader-writer lock with fast concurrent reads.
  - WaitGroup, for synchronizing the beginning or end of some computation.

- Utilities
  - Backoff, for exponential backoff in spin loops.(no_std)
  - CachePadded, for padding and aligning a value to the length of a cache line.(no_std)
  - scope, for spawning threads that borrow local variables from the stack.

borrow 
mpsc
multi producer single consumer

from .. part of rust standard library

- crossbeam-channel provides multi-producer multi-consumer channels for message passing.
- crossbeam-deque provides work-stealing deques, which are primarily intended for building task schedulers.
- crossbeam-epoch provides epoch-based garbage collection for building concurrent data structures.
- crossbeam-queue provides concurrent queues that can be shared among threads.
- crossbeam-utils provides atomics, synchronization primitives, scoped threads, and other utilities.



https://blog.logrocket.com/concurrent-programming-rust-crossbeam/

