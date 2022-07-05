## Defining the data

First the contract's state, what will be written to storage, needs to be defined.

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:10:12}}
```

### `struct`
A `struct` in Rust defines a data structure with fields, similar to a class, object, Hash, or Dictionary in other languages.

### `val`
The field `val` is of type `i8`[^numbers], which, while sounding a bit like a iPhone version, is a signed 8-bit integer. This means it can be between `-128` and `127`. Without [overflow checks](https://doc.rust-lang.org/cargo/reference/profiles.html#overflow-checks) enabled, if `val` is currently `127`, the next time it is incremented it will wrap around to be `-128`.

### `pub`

Lastly, `pub` is short for `public`, meaning that the item is visible outside of the file[^namespaces].

[^numbers]: If you want to read more about number types, check out [The Rust Book's description](https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#integer-types).

[^namespaces]: Technically `pub` is a [namespace](https://doc.rust-lang.org/reference/visibility-and-privacy.html), but that's not important right now.
