# Advanced Message

Having to pass two arguments to create the Message is annoying, why not pass the Message directly!

First let's add some documentation to the struct:

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/advanced/src/lib.rs:24:33}}
```

`Deserialize` and `Serialize` are now needed to allow the `Message` to be parsed to and from JSON.

## Update methods

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/advanced/src/lib.rs:35:45}}
```

Now we have `Message` as a argument to `set_status` and don't need to convert `Message` manually when returning it from `get_status`.
