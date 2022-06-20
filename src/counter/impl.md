# Defining the implementation

To attach methods to a `struct` in Rust, use an `impl`. As a reminder, here's our struct from the last page:

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:10:12}}
```

## A view method

Let's add a method to view the field `val`.

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:15}}
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:17:19}}
    ...
}
```

### `fn`

This is the keyword for `function`. 

### `&self`

This is a reference to the `Counter` struct. This lets you access the fields on the struct, however, it is _immutable_ meaning that you cannot change or "mutate" the field.

The ampersand (`&`) means that the `get_num` function _borrows_ `self`, rather than taking _ownership_ of `self`. To feel confident with Rust, you will need to [understand borrowing and ownership](https://doc.rust-lang.org/stable/book/ch04-00-understanding-ownership.html). For now, pay attention to when this guide uses or omits the ampersand on variable names, and rely on Rust's industry-leading error messages to help you figure out if and when you need an ampersand in your own code.

### `-> i8`

The return type of the method. A signed 8-bit integer, matching `val`.

### `self.val`

This accesses the field `val`. You will also notice that there is no `return` statement. This is because in Rust everything is an expression, and a function returns the value of the last expression, as long as there is no semicolon after it.

## A change method

Next let's see how to mutate or change `val`. 

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:15}}
  ...

{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:22:26}}
  
  ...
}
```

### `;`

Rust uses semicolons to separate expressions. You'll see that the last line here is the same as the only line in `get_num`, with no semicolon. You may have already guessed that this means `set_num` also returns the value of `num`. This also matches the return type, `-> i8`.

### `&mut self`

Here the reference to the struct is defined as mutable, allowing it to be changed.

Mutable references are covered in the same chapter of the Rust Book [on borrowing and ownership](https://doc.rust-lang.org/stable/book/ch04-02-references-and-borrowing.html#mutable-references) linked above.

### `self.val += 1;`

Here the value is incremented by 1.  This is the same as `self.val = self.val + 1`.

### `log!`

This is the first instance of a macro. It's not super important to understand now, but basically it is a special function that generates code.  In this case, `log` generates code that formats a string and emits a log which you can see in [NEAR Explorer](https://explorer.near.org/) or other block-explorer tools.
