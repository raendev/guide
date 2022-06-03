# Basic Counter Example

At its core the NEAR blockchain is a big database.  A contract is a program with its own storage space. You have access to read and write to this storage like you would any key/value data structure.

The simplest example of this is a counter. A contract that can read, increment, and decrement a value.

## Defining the data

First the contract's state, what will be written to storage, needs to be defined.

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:10:12}}
```

A struct in Rust defines a data structure with fields.  You might have seen a class or an object in JS. Unlike other languages, the data structure has no methods attached to it, rather it just defines the shape.

The field `val` is of type `i8`, which, while sounding a bit like a iPhone version, is a signed 8-bit integer meaning it can be between `-128` and `127`.

This means once `val` is incremented to `127`, the next time it will wrap around to be `-128`.


## Defining the implementation

Rust does allow you to attach methods to the data.  This is done with an implementation (`impl`).  Let's add a method to return the field `val`.

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:15:18}}
```
