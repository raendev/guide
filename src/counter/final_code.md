# Final Code

Let's take a look at the full contract and go over the details skipped over first.

## Imports from `near-sdk`
 
```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:5:6}}
```

### Borsh

Borsh is a serialization framework, which deals with converting data into and from bytes. In the case of NEAR it is used for serializing data to be stored in the contract storage. For a type to be Borsh Serializable it needs to implement the `trait`s `BorshSerialize` and `BorshDeserialize`, which are like interfaces, with `serialize` and `deserialize` method respectively.

### log

We saw this before when printing the incremented value. As you can see macros are imported just like everything else.

### `near_bindgen`

This is the real magic of `near-sdk`.  Like `log` it is a macro, but generates code for the whole contract.

## Contract struct

```rust,noplayground,ignore
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:8:12}}
```

`near_bindgen` here means that the contract's state comes from this struct. `derive` is a built in macro. The `Default` trait has a single `default` method, which returns a default instance of a type. `derive(Default)`, generates the code of 

`derive(..., BorshDeserialize, BorshSerialize)` generates an implementation of the traits so that the contract state can be written and read to storage.

<details>
<summary>Generated implementations from `derive` for those interested</summary>

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
{{#include ../../examples/contracts/rust-counter/contract/src/lib.rs:14:38}}
```

Looking again at the view function `get_num` it returns an `i8`, but this data must be serialized so that it can be returned to who ever called this via RPC[^note]. The `near_bindgen` by default generates the code to serialize this into a JSON number since this is the most common case, e.g. a web app calling `get_num`.


[^note]: [Remote Procedure Call]() is the network request made to a NEAR Node to execute a view call or a change call.