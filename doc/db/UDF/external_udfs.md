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

