## Defining the data

First the contract's state, what will be written to storage, needs to be defined.

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:10:12}}
```

### `struct`
A `struct` in Rust defines a data structure with fields.  You might have seen a class or an object in JS. Unlike other languages, the data structure has no methods attached to it, rather it just defines the shape.

### `val`
The field `val` is of type `i8`, which, while sounding a bit like a iPhone version, is a signed 8-bit integer meaning it can be between `-128` and `127`.

This means once `val` is incremented to `127`, the next time it will wrap around to be `-128`.[^numbers]

### `pub`

Lastly, `pub` is short for `public`, meaning that the item is visible outside of the file[^namespaces].

[^numbers]: If you want to read a little more about number types, check out [The Rust Book's description](https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#integer-types).

[^namespaces]: Technically `pub` is a [namespace](https://doc.rust-lang.org/reference/visibility-and-privacy.html), but that's not important right now.
