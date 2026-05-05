# sqlite



https://github.com/polyrand/rust-sqlite-ext-example


## Extending SQLite with Rust

SQLite has a powerful extension mechanism: loadable extensions.

Being an in-process database, SQLite has other extensions mechanisms like application-defined functions (UDF for short). But UDFs have some shortcomings:

- They’re local to an SQLite connection, not shared for every process connected to the DB
- They have to be defined in your program. That means that you need to have the function available in the same scope as your application. This is where loadable extensions come in. Loadable extensions can be written in any programming language that can be compiled to a shared library/DLL. Then you can just share the compiled object and load them from any application or programming language. In this post, we’ll see how we can use Rust to write an SQLite loadable extension.

## Intro
This post is a simplified version of some techniques I learned from phiresky/sqlite-zstd. That is an SQLite extension that enables zstd compression on SQLite, I highly recommend checking it out if you want to look at more advanced examples than this post.


Extension entry point
When you try to load an extension in SQLite, it needs an entry point function. According to the docs of the sqlite3_load_extension C function, if an entry point is not provided, it will attempt to guess one base on the filename. If we called our compiled extension regex_ext it will try to load an entry point called sqlite3_regex_ext_init because the extension has the filename regex_ext.{so,dll,dylib} If you need more flexibility, there’s also an SQL function to load extensions, and it lets you specify the entry point. With that, you can do something like:

```sql
SELECT load_extension('path/to/loadable/extension/regex_ext.[extension]', 'sqlite3_regex_init')
```
Now it will try to find a function called sqlite3_regex_init as an entry point, instead of sqlite3_regex_ext_init.


Let’s write our entry point function:
```rust
#[no_mangle]
pub unsafe extern "C" fn sqlite3_regex_init(
db: *mut ffi::sqlite3,
_pz_err_msg: &mut &mut std::os::raw::c_char,
p_api: *mut ffi::sqlite3_api_routines,
) -> c_int {

    loadable_extension_init(p_api);

    match init(db) {
        Ok(()) => {
            log::info!("[regex-extension] init ok");
            ffi::SQLITE_OK
        }

        Err(e) => {
            log::error!("[regex-extension] init error: {:?}", e);
            ffi::SQLITE_ERROR
        }
    }
}
```

Some details of this function:

- It needs #[no_mangle]: The Rust compiler mangles symbol names differently than native code linkers expect. As such, any function that Rust exports to be used outside of Rust needs to be told not to be mangled by the compiler [source].
- The function is unsafe
- The function returns ffi:SQLITE_OK. In a few paragraphs, we will see how we can change this return code to make the extension persistent across connections.
- The extension is loaded in the init() function, which we’ll go through right now.


[more details](res/Extending%20SQLite%20with%20Rust.pdf)





