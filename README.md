# Serde JSON (allocator_api fork)

**Fork of [serde-rs/json](https://github.com/serde-rs/json) with support for custom allocators via Rust nightly `allocator_api`.**

Adds `from_str_in`, `from_slice_in`, `from_reader_in` — deserialize JSON into arena (e.g. [bumpalo](https://github.com/fitzgen/bumpalo)). Use together with [IvanSenDN/serde](https://github.com/IvanSenDN/serde) (fork with `DeserializeIn` and `#[derive(DeserializeIn)]`).

---

## Requirements

- **Rust 1.93** or newer.
- **Nightly** and `#![feature(allocator_api)]` for the allocator API.

---

## Usage

### Standard (stable)

Same API as upstream. Use this fork via git:

```toml
[dependencies]
serde = { git = "https://github.com/IvanSenDN/serde", features = ["derive"] }
serde_json = { git = "https://github.com/IvanSenDN/serde_json" }
```

### With allocator_api (nightly)

```toml
[dependencies]
serde = { git = "https://github.com/IvanSenDN/serde", features = ["derive", "allocator_api"] }
serde_json = { git = "https://github.com/IvanSenDN/serde_json", features = ["allocator_api"] }
bumpalo = { version = "3", features = ["allocator_api", "collections"] }
```

```rust
#![feature(allocator_api)]

use bumpalo::Bump;
use serde::DeserializeIn;

#[derive(DeserializeIn)]
struct Person<A: std::alloc::Allocator> {
    name: String<A>,  // your arena string type
    age: u8,
}

fn main() {
    let bump = Bump::new();
    let json = r#"{"name": "Alice", "age": 30}"#;
    let person: Person<&Bump> = serde_json::from_str_in(json, &bump).unwrap();
}
```

**API:** `from_str_in`, `from_slice_in`, `from_reader_in` — same as `from_str` / `from_slice` / `from_reader`, but take an allocator and require `T: DeserializeIn<'de, A>`.

---

## Note

Personal-use fork. Development only as needed. Not published to crates.io.

---

Upstream docs: [serde.rs](https://serde.rs/), [docs.rs/serde_json](https://docs.rs/serde_json).

#### License

Licensed under either of [Apache License, Version 2.0](LICENSE-APACHE) or [MIT license](LICENSE-MIT) at your option.
