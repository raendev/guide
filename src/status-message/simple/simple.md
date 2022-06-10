# Contract State

Let's take a look at the struct that defines the contract:

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/simple/src/lib.rs:7:11}}
```

This struct has one field `records`; a `LookupMap` that maps `AccountId`s to `String`s.

You will also notice that this contract doesn't implement `Default`. This is because we are going to implement it ourselves!

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/simple/src/lib.rs:13:19}}
```

This is how you create an implementation that fulfills a trait. In this case there is one function, `default`, which returns `Self`, which is a shorthand for the type implementing the trait, in this case `StatusMessage`.

When the contract executions it first loads the contract's state from storage. If no state exists it calls `StatusMessage::default()` to get the initial instance.

# `new`

You might be familiar with constructors in other languages. Rust has no concept of a constructor, but it is common to have a function called `new` to create an instance of a type.

In this case, `LookupMap::new(b"r")` creates a new `LookupMap`. `b"r"` is byte representation of the string `"r"`, which is used as a prefix before any keys. For example, `eve.testnet` --> `reve.testnet`.  This creates a namespace for this data structure as long as this prefix is unique.