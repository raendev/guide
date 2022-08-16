# Basic Counter Example

At its core, the NEAR blockchain is a big database. A contract is a program with its own storage space, commonly called "contract storage" or "on-chain storage." You have access to read and write to this storage like you would any key/value data structure.

The simplest example of this is a counter. A contract that can read, increment, and decrement a value.

To follow along, use VS Code to open the `contracts/counter` folder inside the `raen-examples` project that you cloned in the last chapter. Then open `src/lib.rs`. We'll walk through this code in order.

## Docs & Imports

The `\\!` lines at the top are [container documentation comments](https://doc.rust-lang.org/stable/book/ch14-02-publishing-to-crates-io.html#commenting-contained-items) you can use to document a whole contract/module/crate.

The `use` lines import dependencies, like `import` or `require` in JavaScript. Look in `Cargo.toml` to see that kebab-case crate names like [`near-sdk`](https://crates.io/crates/near-sdk) are turned into snake-case names like `near_sdk` in the code. Let's learn more about these specific imports by seeing how they're used.

## State Shape

Rust's type system gives us a way to tell NEAR's Runtime how our contract's state is shaped. If you're following along, look for the `struct` definition on line 10:

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:10:12}}
```

About this:

| Thing    | Explanation
|----------|------------
| `struct` | A data structure with fields, similar to a class, object, Hash, or Dictionary in other languages. This `struct` is named `Counter`; you could name it whatever you want.
| `val` | This is a variable name; you could change it (using the `F2` key in VS Code) to anything else you want, such as `counter` or `number` or `num`.
| `i8` | `val` is of type `i8`, a signed 8-bit integer. This means it can be between -128 and 127.[^numbers]
| `pub` | Short for `public`, meaning that the item is visible outside of the file[^namespaces]. You need this on the `struct` you intend to store in contract storage. **WHY** do we need this in addition to `#[near_bindgen]`?

Ok, straightforward enough.

But if you're actually looking at the code in your own editor, you're probably wondering above the stuff right above `pub struct Counter`:

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:8:12}}
```

These are called [macros](https://doc.rust-lang.org/reference/procedural-macros.html). Macros are Rust's way of generating extra code. This kind of "hash bracket" macro (`#[...]`) will expand the definition of the thing that follows, adding extra logic to save you from needing to copy-paste boilerplate.

(The other kind of Rust macro looks like a function, but has an exclamation mark, like `log!(...)`. You'll see this soon.)

Here's what these macros are doing:

| Thing              | Explanation
|--------------------|------------
| `near_bindgen` | Generates [bindings](https://en.wikipedia.org/wiki/Language_binding) for NEAR. This is how you tell the NEAR Runtime that this is the `struct` you want to put in contract storage. **WHAT** code does this generate?
| `derive(...)` | The _[`derive` attribute](https://doc.rust-lang.org/reference/attributes/derive.html)_. Takes a list of [_derive macros_](https://doc.rust-lang.org/reference/procedural-macros.html#derive-macros), and generates extra code for each one.
| `Default` | Generates a default value for the `struct`. If you check the rest of the file, you'll see that we never initialize `Counter` or `val`. But when you deploy the contract, you'll see that `val` defaults to `0`.
| `BorshSerialize` | Generates boilerplate for taking your in-memory contract data and turning it into borsh-serialized bytes to store on-disk.
| `BorshDeserialize` | Generates boilerplate for reading borsh-serialized, on-disk bytes and turning them into in-memory data structures.

**WHY** doesn't `near_bindgen` automatically `derive(BorshDeserialize, BorshSerialize)`? Is it because people may want the flexibility to represent bytes in some other way, or a limitation of Rust macros?

<details>
<summary>Expand this section to see the code the above <code>derive</code> macro will generate.</summary>

```rust,noplayground,ignore
impl ::core::default::Default for Counter {
    #[inline]
    fn default() -> Counter {
        Counter {
            val: ::core::default::Default::default(),
        }
    }
}
impl borsh::de::BorshDeserialize for Counter
where
    i8: borsh::BorshDeserialize,
{
    fn deserialize(buf: &mut &[u8]) -> ::core::result::Result<Self, borsh::maybestd::io::Error> {
        Ok(Self {
            val: borsh::BorshDeserialize::deserialize(buf)?,
        })
    }
}
impl borsh::ser::BorshSerialize for Counter
where
    i8: borsh::ser::BorshSerialize,
{
    fn serialize<W: borsh::maybestd::io::Write>(
        &self,
        writer: &mut W,
    ) -> ::core::result::Result<(), borsh::maybestd::io::Error> {
        borsh::BorshSerialize::serialize(&self.val, writer)?;
        Ok(())
    }
}
```

</details>

## Borsh?

The data in your contract and, indeed, your contract itself, need to be stored on-disk in between calls to your contract.

Then when a NEAR user makes a call to your contract, [validators'](https://docs.near.org/concepts/basics/validators) computers need to fetch your contract from storage, load it into memory, check how your contract's storage is shaped, then finally deserialize your contract storage and execute the call.

[Borsh](https://borsh.io/) is a serialization specification (a way of representing data as bytes) developed and maintained by the nearcore team, because nothing else was good enough when NEAR started. The biggest problem with other serialization standards was a lack of consistency—the same in-memory object could be represented in multiple valid ways as bytes.

You'll see Borsh used in a couple different ways in NEAR contracts. The most common is here—as the serialization format for your contract data.

## Methods

To attach methods to a `struct` in Rust, use an `impl`.

### A View Method

Let's start by looking at the view method starting on line 17:

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:15:19}}
    ...
}
```

| Thing      | Explanation
|------------|------------
| `///`      | In Rust, you can use `//` for regular comments and `///` for [documentation comments](https://doc.rust-lang.org/stable/book/ch14-02-publishing-to-crates-io.html#making-useful-documentation-comments), which will automatically be turned into documentation by various tooling, including RAEN.
| `fn`       | The keyword for `function`
| `&self`    | A reference to the `Counter` struct. This lets you access the fields on the struct, however, it is _immutable_ meaning that you cannot change or "mutate" the field.<br><br>The ampersand (`&`) means that the `get_num` function _borrows_ `self`, rather than taking _ownership_ of `self`. To feel confident with Rust, you will need to [understand borrowing and ownership](https://doc.rust-lang.org/stable/book/ch04-00-understanding-ownership.html). For now, pay attention to when this guide uses or omits the ampersand on variable names, and rely on Rust's industry-leading error messages to help you figure out if and when you need an ampersand in your own code.
| `-> i8`    | The return type of the method. A signed 8-bit integer, matching `val`.
| `self.val` | This accesses the field `val`. You will also notice that there is no `return` statement. This is because in Rust everything is an expression, and a function returns the value of the last expression, as long as there is no semicolon after it.

Go ahead and play with this a bit. Do things that don't make sense, and your well-configured editor will yell at you. Looking at the errors will teach you about how Rust works. You can try each of these things, changing it back to how it was after each:

* Change `-> i8` to `-> i16`
* Change `-> i8` to `-> ()`, an empty [tuple](https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#the-tuple-type)
* Remove `-> i18` – note that you get the same error message as when you changed it to an empty tuple! Empty tuple is one of the closest things Rust has no a nothing/`null` type.
* Change `self.val` to `self.val;`
* Change `self.val` to `return self.val;` and it will behave the same way as `self.val` with no semicolon. Note that this is super non-idiomatic! No one writes Rust this way. Get used to paying attention to the presence & lack of semicolons.

### A Change Method

Next let's see how to mutate or change `val`. Look at the `increment` method starting on line 22:

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:15}}
    ...

{{#include ../../examples/contracts/counter/src/lib.rs:21:26}}
  
    ...
}
```

| Thing      | Explanation
|------------|------------
| `;` | Rust uses semicolons to separate expressions. See that the last line here is the same as the only line in `get_num`, with no semicolon. You may have already guessed that this means `set_num` also returns the value of `num`. This also matches the return type, `-> i8`.
| <nobr>`&mut self`</nobr> | Here the reference to the struct is defined as mutable, allowing it to be changed. <br><br>Mutable references are covered in that same chapter of the Rust Book [on borrowing and ownership](https://doc.rust-lang.org/stable/book/ch04-02-references-and-borrowing.html#mutable-references).
| <nobr>`self.val += 1;`</nobr> | The same as `self.val = self.val + 1`. This mutates the `Counter` struct. Note that the new value will not be saved to contract storage until the method call is over. If you have a bug in the `log!` line or before the return line, the new value will not actually be persisted.
| `log!` | This is a function-like macro, as mentioned above. `log!` generates code that formats a string and emits a log which you can see in [NEAR Explorer](https://explorer.near.org/) or other block-explorer tools.

## Digging Deeper: External Interface Serialization

Look again at the view function `get_num`. It returns an `i8`. However, any call to a NEAR contract happens via [Remote Procedure Calls](https://docs.near.org/docs/api/overview#rpc-api), and this `i8` needs to be serialized into something the consumer can read. By default, `near_bindgen` generates code to serialize it into a JSON number, since this is the most common case, e.g. a web app calling `get_num`. If you really need to, you can override this setting. For example, this would serialize it to Borsh instead:

```rust,noplayground,ignore
#[result_serializer(borsh)]
pub fn get_num(&self) -> i8 {
    self.val
}
```

Serializing to Borsh can be useful in a couple cases:

* [gas](https://docs.near.org/docs/concepts/gas)-intensive contract methods that need to optimize every detail
* methods only used in [cross-contract call](https://docs.near.org/docs/tutorials/contracts/cross-contract-calls) logic, where no external consumers are expected or allowed

However, most of the time you will probably want to stick with the default JSON result serialization, to make it easier for apps to interact with your contract.

## Summary

In this chapter we saw our first NEAR smart contract. We were introduced to some Rust basics, like its type system, numbers, `struct`, documentation comments, and (im)mutability. We saw how NEAR works with Rust to generate code for you using _macros_, leaving the Rust code itself clean and readable.

Next, we'll build the contract with `raen` and interact with it using RAEN Admin.

[^numbers]: For more on Rust's number types, check out [The Rust Book's description](https://doc.rust-lang.org/stable/book/ch03-02-data-types.html#integer-types). For more on why `i8` goes from negative 128 but only to positive 127, search for primers about [how numbers get stored as binary](https://www.electronics-tutorials.ws/binary/signed-binary-numbers.html).

[^namespaces]: Technically `pub` is a [namespace](https://doc.rust-lang.org/reference/visibility-and-privacy.html), but that's not important right now.
