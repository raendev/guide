# Defining the implementation

Rust does allow you to attach methods to the data.  This is done with an implementation (`impl`).  

## A `view` method

Let's add a method to `view` the field `val`.

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:15}}
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:17:19}}
  ...
}
```

### `fn`

This is the keyword for `function`. 

### `&self`

This is the reference to the `Counter` struct. This lets you access the fields on the struct, however, it is _immutable_ meaning that you cannot change or "mutate" the field.

### `-> i8`

This is the return type of the method, here an signed 8-bit integer, the same as `val`.

### `self.val`

This accesses the field `val`.  You will also notice that there is no `return` statement, this is because in rust everything is an expression and a function returns the value of the last expression.

You might be familiar with `/** Doc Strings */` in other languages, Rust uses `///` vs `//` to attach the comment to document something.


## A `change` method

Next let's see how to mutate or change `val`. 

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:15}}
  ...

{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:22:25}}
  
  ...
}
```

### `&mut self`

Here the reference to the struct is defined as mutable, allowing it to be changed.

### `self.val += 1;`

Here the value is incremented by 1.  This is the same as `self.val = self.val + 1`. You will also notice the semicolon, `;`; this is to separate the expressions.

### `log!`

This is the first instance of a macro. It's not super important to understand now, but basically it is a special function that generates code.  In this case `log` generates code that formats a string and emits a log from executing the function which you can see in the NEAR explorer.

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:24}}
```
