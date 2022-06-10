# Updating the Methods

Now we need to update our methods so that we can pass two `String`s instead of just one.


## `set_status`

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/intermediate/src/lib.rs:32:35}}
```

One thing to note is `Message { title, body }`, this is how you can directly create a message without using a `new` function like we saw earlier.

## `get_status`

```rust,noplayground,ignore
{{#include ../../../examples/contracts/status-message/contracts/intermediate/src/lib.rs:37:41}}
```

Before `account_id` was a `String`, but now we can use `AccountId` directly which will panic the contract if the account id is not valid.

Next we have a new function [map](https://doc.rust-lang.org/std/option/enum.Option.html#method.map), which is a method `Option`. And a new syntax `|Message { body, title}|`. 

`map` is a function takes a function as argument. Then if the Option is not `None` it will pass the inner value to the function. In Rust an anonymous rust has the following syntax, `|x| x + 1`, if you are familiar with JavaScript it is like an arrow function `(x) => x + 1`.

Rust let's you ~destructure~ the argument passed to the function.

```rust
|Message { body, title }| format!(r#"{{"body":"{body}", "title": "{title}"}}"#))

// Is the same as the function
|msg| {
  let body = msg.body;
  let title = msg.title;
  format!(r#"{{"body":"{body}", "title": "{title}"}}"#)
})
```

Lastly, we are creating a String that represents the JSON representation of `Message`, later we won't have to do this by hand.

`format!` is a macro for injecting strings into another in this case `{body}` and `{title}` are replaced by the corresponding variables.

`r#".."#` is called a raw string, which let's us use a `"` from within the string. And we need two `{{` and `}}` so that `format!` understands that it's actually a `{` or `}` in the `String`.
