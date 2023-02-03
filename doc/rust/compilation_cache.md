# sccache - Shared Compilation Cache

https://github.com/mozilla/sccache/

sccache is a ccache-like compiler caching tool. It is used as a compiler wrapper and avoids compilation when possible, storing cached results either on local disk or in one of several cloud storage backends.

sccache includes support for caching the compilation of C/C++ code, Rust, as well as NVIDIA's CUDA using nvcc.



## Usage

Running sccache is like running ccache: prefix your compilation commands with it, like so:

```sh
sccache gcc -o foo.o -c foo.c
```

If you want to use sccache for caching Rust builds you can define build.rustc-wrapper in the cargo configuration file. For example, you can set it globally in $HOME/.cargo/config.toml by adding:

```yaml
[build]
rustc-wrapper = "/path/to/sccache"
```

Note that you need to use cargo 1.40 or newer for this to work.

Alternatively you can use the environment variable RUSTC_WRAPPER:

```shell
export RUSTC_WRAPPER=/path/to/sccache
cargo build
```

sccache supports gcc, clang, MSVC, rustc, NVCC, and Wind River's diab compiler.

If you don't specify otherwise, sccache will use a local disk cache.




