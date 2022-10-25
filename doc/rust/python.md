# PUFF


https://github.com/hansonkd/puff

It's an experiment to minimize the barrier between Python and Rust to unlock the full potential of high level languages. Build your own Runtime using standard CPython and extend it with Rust. Imagine if GraphQL, Postgres, Redis and PubSub were part of the standard library. That's Puff.

High level overview is that Puff gives Python

- Greenlets on Rust's Tokio.
- High performance HTTP Server - combine Axum with Python WSGI apps (Flask, Django, etc.)
- Rust / Python natively in the same process, no sockets or serialization.
- AsyncIO / uvloop / ASGI integration with Rust
- An easy-to-use GraphQL service
- Multi-node pub-sub
- Rust level Redis Pool
- Rust level Postgres Pool
- Websockets
- semi-compatible with Psycopg2 (hopefully good enough for most of Django)
- A safe convenient way to drop into rust for maximum performance


