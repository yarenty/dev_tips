# rustix
Safe Rust bindings to POSIX/Unix/Linux/Winsock2 syscalls
https://crates.io/crates/rustix


A Bytecode Alliance project

Github Actions CI Status zulip chat crates.io page docs.rs docs

rustix provides efficient memory-safe and I/O-safe wrappers to POSIX-like, Unix-like, Linux, and Winsock2 syscall-like APIs, with configurable backends. It uses Rust references, slices, and return values instead of raw pointers, and io-lifetimes instead of raw file descriptors, providing memory safety, I/O safety, and provenance. It uses Results for reporting errors, bitflags instead of bare integer flags, an Arg trait with optimizations to efficiently accept any Rust string type, and several other efficient conveniences.

rustix is low-level and, and while the net API supports Winsock2 on Windows, the rest of the APIs do not support Windows; for higher-level and more portable APIs built on this functionality, see the system-interface, cap-std, and fs-set-times crates, for example.

rustix currently has two backends available:

linux_raw, which uses raw Linux system calls and vDSO calls, and is supported on Linux on x86-64, x86, aarch64, riscv64gc, powerpc64le, arm (v5 onwards), mipsel, and mips64el, with stable, nightly, and 1.48 Rust.

By being implemented entirely in Rust, avoiding libc, errno, and pthread cancellation, and employing some specialized optimizations, most functions compile down to very efficient code. On nightly Rust, they can often be fully inlined into user code.
Most functions in linux_raw preserve memory, I/O safety, and pointer provenance all the way down to the syscalls.
libc, which uses the libc crate which provides bindings to native libc libraries on Unix-family platforms, and windows-sys for Winsock2 on Windows, and is portable to many OS's.


# same-file
A safe and cross platform crate to determine whether two files or directories are the same.



# inout
Custom reference types for code generic over in-place and buffer-to-buffer modes of operation.




# filetime

A helper library for inspecting and setting the various timestamps of files in Rust. This library takes into account cross-platform differences in terms of where the timestamps are located, what they are called, and how to convert them into a platform-independent representation.

**Advantages over using std::fs::Metadata**

This library includes the ability to set this data, which std does not.

This library, when built with RUSTFLAGS=--cfg emulate_second_only_system set, will return all times rounded down to the second. This emulates the behavior of some file systems, mostly HFS, allowing debugging on other hardware.



# os_pipe.rs 

A cross-platform library for opening OS pipes, like those from pipe on Linux or CreatePipe on Windows. The Rust standard library provides Stdio::piped for simple use cases involving child processes, but it doesn't support creating pipes directly. This crate fills that gap.



# num_cpus

Get the number of CPUs on a machine.



# io-lifetimes
A low-level I/O ownership and borrowing library


This library introduces OwnedFd, BorrowedFd, and supporting types and traits, and corresponding features for Windows, which implement safe owning and borrowing I/O lifetime patterns.

This is associated with RFC 3128, the I/O Safety RFC, which is now merged. Work is now underway to move the OwnedFd and BorrowedFd types and AsFd trait developed here into std.

Some features currently require nightly Rust, as they depend on rustc_attrs to perform niche optimizations needed for FFI use cases.