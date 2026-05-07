---
title: Sleep sort
main_link: https://en.wikipedia.org/wiki/Sleep_sort
keywords: [sleepsort, sorting, async, scheduler, asyncio, joke-algorithm, 4chan]
status: reviewed
review_date: 2026/05/03
---

# Sleep sort

**Main link:** <https://en.wikipedia.org/wiki/Sleep_sort>

## Summary

A joke sorting algorithm posted on 4chan in 2011: spawn one async task per input number, each sleeps for `n` time units, then writes itself to a shared list. Smaller numbers wake up first, so the list ends up sorted. The "compute" is delegated to the OS scheduler. The runtime is **O(max-value)**, not O(n log n), and depends entirely on the precision and load of the system clock — but it's a delightful illustration of how async / scheduling primitives can substitute for traditional comparisons.

## Insight

Don't ship it. Do read it: sleep sort is the most *vivid* one-page demonstration of (a) why async/await is concurrency rather than parallelism, (b) how OS schedulers handle co-pending timers, and (c) how an algorithm's complexity can sit in the *environment* rather than the code. Useful as a teaching aid for `asyncio`/`Promise.all`/`tokio::join!` semantics. The "speed-up" trick — divide each delay by some scale factor — is the same insight behind real time-bucket data structures (timing wheels, calendar queues).

Two real-world limitations to call out: (1) precision of `sleep` clamps it to ~1 ms on most kernels, so the inputs must already be sortable by their integer ranks at that resolution; (2) it cannot sort negative numbers without extra work.

## Similar / related topics

- Bogosort, stooge sort, sleep sort — the canonical "joke sort" trio.
- Timing wheels (Hashed Timing Wheels paper, Kafka's `Time Wheel`) — practical use of the same "schedule-by-delay" mechanism.
- `asyncio.gather` / `tokio::join!` / `Promise.all` — concurrency primitives sleep sort relies on.
- Scheduler theory in general — see `man 2 nanosleep`, CFS / EEVDF in Linux.
- [[algorithms]] — Curated catalogue of esoteric Brainfuck algorithms (different kind of joke).

## Internal links

- [[brainfuck]] — Other "joke / educational" content in this section.
- [[algorithms]] — Brainfuck algorithm catalogue (sibling esoteric-programming entry).

## Keywords

`#sleepsort` `#sorting` `#async` `#scheduler` `#asyncio` `#joke-algorithm` `#4chan`

## References / raw notes

- Wikipedia: <https://en.wikipedia.org/wiki/Sleep_sort>
- Animesh Chouhan's writeup with Python `asyncio` example: <https://animeshchouhan.com/posts/sleepsort/>
- Original 4chan thread (via Princeton lecture slides): <https://www.cs.princeton.edu/courses/archive/fall13/cos226/lectures/52Tries.pdf>
- Hacker News discussion: <https://news.ycombinator.com/item?id=2657277>
- JavaScript gist: <https://gist.github.com/Thatkookooguy/3b7ce09a9f80a5e6541175104a5d49e9>
- Python `asyncio` docs: <https://docs.python.org/3/library/asyncio.html>

### Reference Python implementation (with `asyncio`)

```python
import asyncio

async def sleepsort(nums):
    sorted_nums = []

    async def add_num(num):
        await asyncio.sleep(num)
        sorted_nums.append(num)

    await asyncio.gather(*(add_num(n) for n in nums))
    return sorted_nums

async def main():
    print(await sleepsort([3, 2, 1, 0, 5, 4]))

asyncio.run(main())
# -> [0, 1, 2, 3, 4, 5]
```

### "Faster" variant — scale the delays

Divide each delay by a constant `scale` so the wall-clock time is `max(input)/scale` instead of `max(input)` seconds. The relative *order* of wake-ups is preserved:

```python
SCALE = 1e3   # treat each unit as 1 ms instead of 1 s

async def sleepsort_fast(nums):
    sorted_nums = []
    async def add_num(num):
        await asyncio.sleep(num / SCALE)
        sorted_nums.append(num)
    await asyncio.gather(*(add_num(n) for n in nums))
    return sorted_nums
```

For 1000 reverse-sorted inputs at `SCALE=1e3`: ~1.0 s end-to-end (vs Python's built-in `sorted()` at ~57 µs). The cost of the joke is roughly **17 000×** the cost of doing it properly.

### Why it's not actually a sort

- **Stability** — depends on insertion-order ties at the scheduler level; not guaranteed.
- **Determinism** — heavily dependent on system load and clock precision (`nanosleep` accuracy).
- **Negative numbers** — unsupported without an offset preprocessing step.
- **Memory** — O(n) tasks, each holding closure state until they wake. Not free.
