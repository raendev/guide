# Contract Methods

In this case we will just have two methods, `set_status` and `get_status`.

## `set_status`

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/simple/src/lib.rs:23:26}}
```

Unlike the counter this method takes an argument, `meassage` which is a `String`. This message is then inserted into the map, which writes it to storage. You'll notice a `&`, [which learned about in the previous chapter](../../counter/impl.md#self), this is how we pass a reference to `insert`, which will then be able to read the `String` to write it to storage.

## `get_status`

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/simple/src/lib.rs:28:30}}
```

Here is our first example of an `Option` type. An Option is other some value or nothing, `Option::Some(value), Option::None`. This is the same as `null` in other languages. `records.get` will return `None` if the account was not in the map. This function will then return `None`.

We also need to convert the string `account_id` to an `AccountId`. There is a `parse` method, which returns will potentially fail if the `account_id` is invalid. In rust this is a `Result`, which either has a value or an error. `unwrap` let's us access the value and will panic and exit if there was an error.
