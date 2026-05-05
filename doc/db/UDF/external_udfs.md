---
title: External UDFs
main_link: https://lib.rs/crates/dlopen2
keywords: [external-udfs, udf, ffi, rust, abi, programming, lib, arrow]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# External UDFs

**Main link:** <https://lib.rs/crates/dlopen2>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[hive]] — Hive UDF – User Defined Function with Example _(score 19.9)_
- [[spark]] — Spark UDF _(score 19.9)_
- [[mariadb]] — MariaDB _(score 19.9)_
- [[debug]] — Debug _(score 13.2)_
- [[rtic]] — RTIC _(score 13.2)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#external-udfs` `#udf` `#db` `#ffi` `#rust` `#abi` `#shared`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# External UDFs 

**This is just a draft idea**, but in general it should work 
and it should not have same problems like previous approach where 
we had issues with rust ABI instability

it is based on: 

https://lib.rs/crates/dlopen2
https://lib.rs/crates/dlopen2_derive

which are used to interact with shared libraries. 

all interaction between datafusion and shared library method has been done using arrow FFI interface

https://docs.rs/arrow/latest/arrow/ffi/index.html

all interaction between rust code and shared library is performed using standard C ABI

it should be a zero copy approach with no serialization overhead. 

```rust
#[derive(Clone)]
pub struct UdfDylib {
    name: Arc<String>,
    signature: Arc<Signature>,
    return_type: Arc<DataType>,
    udf: Arc<Container<UdfDylibInterface>>,
}

#[derive(WrapperApi)]
struct UdfDylibInterface {
    run: unsafe extern "C-unwind" fn(
        args_ptr: *mut FfiArraySchemaPair,
        args_len: usize,
        args_capacity: usize,
    ) -> FfiArrayResult,

#[derive(Clone)]
#[derive(Debug)]
struct FfiArraySchemaPair(FFI_ArrowArray, FFI_ArrowSchema);

#[repr(C)]
pub struct FfiArrayResult(FFI_ArrowArray, FFI_ArrowSchema, bool); // bool was the result walid
```
shared library function exposes standard C interface  

```rust
pub extern "C-unwind" fn run(args_ptr: *mut FfiArraySchemaPair, args_len: usize, args_capacity: usize) -> FfiArrayResult {..}
```

but under the hood could be implemented in rust or any other language which understands C interaction. 

if implemented in rust, a macro could be provided which would do translation between FFI arrays and local arrays. 

NOTE: there is a lot of `unsafe` in this implementation, thus it should be implemented with extra care.
