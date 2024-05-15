# Alloy 

RUST

https://github.com/alloy-rs/alloy 



Alloy connects applications to blockchains.

Alloy is a rewrite of ethers-rs from the ground up, with exciting new features, high performance, and excellent docs.

ethers-rs will continue to be maintained until we have achieved feature-parity in Alloy. No action is currently needed from devs.


## Installation
Currently, Alloy is not hosted on crates.io, the Rust package registry.

To incorporate Alloy into your project, you will need to specify the GitHub repository as the source. This can be achieved by executing the following command in your terminal:

cargo add alloy --git https://github.com/alloy-rs/alloy
After incorporating Alloy, you may wish to utilize specific features of the crate. These features can be enabled through modifications in your project's Cargo.toml file. A comprehensive list of available features can be found at this GitHub link.

## Overview
This repository contains the following crates:

alloy: Meta-crate for the entire project, including alloy-core
alloy-consensus - Ethereum consensus interface
alloy-contract - Interact with on-chain contracts
alloy-eips - Ethereum Improvement Proposal (EIP) implementations
alloy-genesis - Ethereum genesis file definitions
alloy-json-rpc - Core data types for JSON-RPC 2.0 clients
alloy-network - Network abstraction for RPC types
alloy-node-bindings - Ethereum execution-layer client bindings
alloy-provider - Interface with an Ethereum blockchain
alloy-pubsub - Ethereum JSON-RPC publish-subscribe tower service and type definitions
alloy-rpc-client - Low-level Ethereum JSON-RPC client implementation
alloy-rpc-types - Ethereum JSON-RPC types
alloy-rpc-types-engine - Ethereum execution-consensus layer (engine) API RPC types
alloy-rpc-types-trace - Ethereum RPC trace types
alloy-signer - Ethereum signer abstraction
alloy-signer-aws - AWS KMS signer implementation
alloy-signer-gcp - GCP KMS signer implementation
alloy-signer-ledger - Ledger signer implementation
alloy-signer-trezor - Trezor signer implementation
alloy-signer-wallet - Local wallet (Keystore/Mnemonic/Yubihsm) signer implementation
alloy-transport - Low-level Ethereum JSON-RPC transport abstraction
alloy-transport-http - HTTP transport implementation
alloy-transport-ipc - IPC transport implementation
alloy-transport-ws - WS transport implementation