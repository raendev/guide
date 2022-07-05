# Final Code

Let's take a look at the full contract and go over details we skipped at first.

## Imports from `near-sdk`
 
```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:5:6}}
```

### Borsh

Borsh is [a serialization framework](https://borsh.io/), which deals with converting data into and from bytes. In the case of NEAR it is used for serializing data to be stored in the contract storage. For a Rust type to be Borsh Serializable it needs to implement `BorshSerialize` and `BorshDeserialize`.

### log

We saw this before when printing the incremented value. As you can see macros are imported just like everything else.

### `near_bindgen`

This is the real magic of `near-sdk`.  Like `log` it is a macro, but generates code for the whole contract.

## Contract struct

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:8:12}}
```

`near_bindgen` here means that the contract's state comes from this struct.

`derive` is a built-in macro. Each item passed to `derive` is a [trait](https://doc.rust-lang.org/stable/book/ch10-02-traits.html):

* The `Default` trait has a single `default` method, which returns a default instance of a type.
* `BorshDeserialize` generates a `deserialize` method, so contract state can be read from storage.
* `BorshSerialize` generates a `serialize` method, so contract state can be written to storage.

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


## Full implementation

```rust,noplayground,ignore
{{#include ../../examples/contracts/counter/src/lib.rs:14:34}}
```

In Rust, you can use `//` for regular comments and `///` for [documentation comments](https://doc.rust-lang.org/stable/book/ch14-02-publishing-to-crates-io.html#making-useful-documentation-comments), which will automatically be turned into documentation by various tooling.

## Digging deeper

Look again at the view function `get_num`. It returns an `i8`. However, any call to a NEAR contract happens via [Remote Procedure Calls](https://docs.near.org/docs/api/overview#rpc-api), and this `i8` needs to be serialized into something the consumer can read. By default, `near_bindgen` generates code to serialize it into a JSON number, since this is the most common case, e.g. a web app calling `get_num`. If you really need to, you can override this setting. For example, this would serialize it to Borsh instead:

```rust,noplayground,ignore
#[result_serializer(borsh)]
pub fn get_num(&self) -> i8 {
    self.val
}
```

This can be useful in a couple cases:

* [gas](https://docs.near.org/docs/concepts/gas)-intensive contract methods that need to optimize every detail
* methods only used in [cross-contract call](https://docs.near.org/docs/tutorials/contracts/cross-contract-calls) logic, where no external consumers are expected or allowed

However, most of the time you will probably want to stick with the default JSON result serialization, to make it easier for apps to interact with your contract.
