# ruMQTT

https://github.com/bytebeamio/rumqtt


rumqtt is an opensource set of libraries written in rust-lang to implement the MQTT standard while striving to be simple, robust and performant.


| Crate |	Description	|
|----|----|
| rumqttc	| A high level, easy to use mqtt client	|
| rumqttd	| A high performance, embeddable MQTT broker |	


## NATIVE!


# paho MQTT


https://crates.io/crates/paho-mqtt

The Eclipse Paho MQTT Rust client library on memory-managed operating systems such as Linux/Posix, Mac, and Windows.

The Rust crate is a safe wrapper around the Paho C Library.

## Features
The initial version of this crate is a wrapper for the Paho C library, and includes all of the features available in that library, including:

Supports MQTT v5, 3.1.1, and 3.1
Network Transports:
Standard TCP support
SSL / TLS (with optional ALPN protocols)
WebSockets (secure and insecure), and optional Proxies
QoS 0, 1, and 2
Last Will and Testament (LWT)
Message Persistence
File or memory persistence
User-defined key/value persistence (including example for Redis)
Automatic Reconnect
Offline Buffering
High Availability
Several API's:
Async/Await with Rust Futures and Streams for asynchronous operations.
Traditional asynchronous (token/wait) API
Synchronous/blocking API