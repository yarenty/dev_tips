
# GRPC

## libs

https://github.com/grpc-ecosystem/awesome-grpc

## Grip - MacOS native client

https://gripgrpc.dev/

A native gRPC client for macOS.

## rust




### tonic - currently most used (2023/03)

https://github.com/hyperium/tonic

A rust implementation of gRPC, a high performance, open source, general RPC framework that puts mobile and HTTP/2 first.

tonic is a gRPC over HTTP/2 implementation focused on high performance, interoperability, and flexibility. This library was created to have first class support of async/await and to act as a core building block for production systems written in Rust.


!!!
https://github.com/hyperium/tonic/blob/master/examples/src/helloworld/server.rs








### grpc-rs

https://github.com/tikv/grpc-rs

_from tikv_

gRPC-rs is a Rust wrapper of gRPC Core. gRPC is a high performance, open source universal RPC framework that puts mobile and HTTP/2 first.



## links

- https://medium.com/geekculture/quick-start-to-grpc-using-rust-c655785fc6f4 - this one is quite good - done

- https://dev.to/alexmercedcoder/understanding-rpc-tour-of-api-protocols-grpc-nodejs-walkthrough-and-apache-arrow-flight-55bd

- https://grpc.io/docs/languages/


## extra


### raft - consensus algorithm

https://github.com/tikv/raft-rs

When building a distributed system one principal goal is often to build in fault-tolerance. That is, if one particular node in a network goes down, or if there is a network partition, the entire cluster does not fall over. The cluster of nodes taking part in a distributed consensus protocol must come to agreement regarding values, and once that decision is reached, that choice is final.

Distributed Consensus Algorithms often take the form of a replicated state machine and log. Each state machine accepts inputs from its log, and represents the value(s) to be replicated, for example, a hash table. They allow a collection of machines to work as a coherent group that can survive the failures of some of its members.

Two well known Distributed Consensus Algorithms are Paxos and Raft. Paxos is used in systems like Chubby by Google, and Raft is used in things like tikv or etcd. Raft is generally seen as a more understandable and simpler to implement than Paxos.