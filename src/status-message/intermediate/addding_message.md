# Adding a new `Message` struct

We just say how to make a message that was a `String`, but that is a little boring. Let's make a `Message` with a body and a title.

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/intermediate/src/lib.rs:25:28}}
```

Next we need to update contract's state struct.

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/intermediate/src/lib.rs:11:13}}
```
