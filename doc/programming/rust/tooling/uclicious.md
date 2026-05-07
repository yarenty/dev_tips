---
title: uclicious — UCL (libucl) config parser with derive support
main_link: https://github.com/andoriyu/uclicious
keywords: [uclicious, rust, ucl, libucl, config, derive, parser, nginx-style]
status: reviewed
---

# uclicious — UCL (libucl) config parser with derive support

**Main link:** <https://github.com/andoriyu/uclicious>

## Summary

`uclicious` is a Rust binding + derive macro for [libUCL](https://github.com/vstakhov/libucl), the **Universal Configuration Library** behind FreeBSD's `pkgng`, rspamd, and a handful of other BSD-world projects. UCL syntax is a nginx-style superset of JSON: implicit arrays, sections without commas, units (`1KB`, `10ms`), `include`s, and macros. `uclicious` lets you derive a typed Rust builder from a `#[derive(Uclicious)]` struct, mapping UCL keys to fields with optional defaults, validators, and type-mapping.

## Insight

Reach for `uclicious` only if you specifically need to read UCL — i.e. you're talking to one of the projects above, or you're personally fond of nginx-shaped configs. For a new project, the conventional Rust paths are:

- **TOML** via [`serde` + `toml`](https://crates.io/crates/toml) — the default for Cargo and most Rust apps.
- **YAML** via [`serde-yaml`](https://crates.io/crates/serde-yaml) (now `serde_yaml` deprecated; consider `serde_yml`) for K8s-shaped configs.
- **JSON** via [`serde_json`](https://crates.io/crates/serde_json) for machine consumption.
- **[`config`](https://crates.io/crates/config)** crate as a layered loader (file + env + CLI overrides).
- **[`figment`](https://crates.io/crates/figment)** as a more flexible layered config story (used by Rocket).

UCL's killer features over JSON are **comments, units, implicit arrays, includes, and macros** — all useful, none unique. TOML covers comments + types; YAML covers all of UCL's surface (and more, painfully); HCL (Terraform) covers the nginx-shaped block syntax with broader tooling. The closest pure equivalent in Rust-land is probably **JSONNet** (via the `jrsonnet` crate) for templated configs.

The library's strength is the `derive` ergonomics — see the example in raw notes below — which work nicely once you've decided UCL is the right format. Validators, type-mapping (`from`/`try_from`/`from_str`/`map`), default values, and per-field path attributes are all there. License is BSD-2-Clause.

**When to pick `uclicious`**: integrating with an existing UCL-emitting tool (rspamd, FreeBSD package configs); you want nginx-shaped configs without writing a parser. **Otherwise**: pick TOML + serde.

## Similar / related topics

- [TOML](https://toml.io/) + [`serde`](https://serde.rs/) — the Rust default config format.
- [YAML](https://yaml.org/) + `serde_yml` — for K8s-shaped configs.
- [HCL](https://github.com/hashicorp/hcl) — HashiCorp's nginx-shaped DSL; closer to UCL.
- [`config`](https://crates.io/crates/config) — layered config loader (file + env + CLI).
- [`figment`](https://crates.io/crates/figment) — Rocket's flexible config layered loader.
- [`jrsonnet`](https://crates.io/crates/jrsonnet) — Rust JSONNet (templated configs).

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[../io/json|io/json]] — JSON / serialisation in Rust.

## Keywords

`#uclicious` `#rust` `#ucl` `#libucl` `#config` `#derive` `#parser`

## References / raw notes

- Repo: <https://github.com/andoriyu/uclicious>
- Built on top of [libucl](https://github.com/vstakhov/libucl) (the FreeBSD/rspamd config library).
- Internal parser supports both nginx-like and JSON-like formats; every JSON file is a valid UCL file (but not vice versa).

### Raw API example

```rust
use uclicious::*;
let mut parser = Parser::default();
let input = r#"
test_string = "no scope"
a_float = 3.14
an_integer = 69420
is_it_good = yes
buffer_size = 1KB
interval = 1s
"#;
parser.add_chunk_full(input, Priority::default(), DEFAULT_DUPLICATE_STRATEGY).unwrap();
let result = parser.get_object().unwrap();

assert_eq!(result.lookup("test_string").unwrap().as_string().unwrap(), "no scope");
assert_eq!(result.lookup("a_float").unwrap().as_f64().unwrap(), 3.14);
assert_eq!(result.lookup("an_integer").unwrap().as_i64().unwrap(), 69420);
assert_eq!(result.lookup("is_it_good").unwrap().as_bool().unwrap(), true);
assert_eq!(result.lookup("buffer_size").unwrap().as_i64().unwrap(), 1024);
```

### Derive example (excerpt)

```rust
use uclicious::*;
use std::path::PathBuf;
use std::net::SocketAddr;
use std::collections::HashMap;
use std::time::Duration;

#[derive(Debug, Uclicious)]
#[ucl(var(name = "test", value = "works"))]
struct Connection {
    #[ucl(default)]
    enabled: bool,
    host: String,
    #[ucl(default = "420")]
    port: i64,
    buffer: u64,
    #[ucl(path = "type")]
    kind: String,
    locations: Vec<PathBuf>,
    addr: SocketAddr,
    extra: Extra,
    #[ucl(path = "subsection.host")]
    hosts: Vec<String>,
    #[ucl(default)]
    option: Option<String>,
    gates: HashMap<String, bool>,
    interval: Duration,
}
```

### Supported field-level attributes (summary)

- `default` / `default = expression` — fallback value.
- `path = "..."` — UCL key (defaults to field name; dot notation supported).
- `validate = path::to_method` — `Fn(key, value) -> Result<(), ObjectError>`.
- `from = Type` / `try_from = Type` / `from_str` — type conversion via `From` / `TryFrom` / `FromStr`.
- `map = path::to_method` — custom `Fn(ObjectRef) -> Result<T, ObjectError>` for everything else.
- `skip_builder` (struct level) — disable builder method generation.

For the full attribute reference (parser flags, var, include, pre_source_hook, etc.) see the upstream README.
