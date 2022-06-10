# Basic Status Message Contract

Previously the contract had state that was shared by any account that can call the contract. Next we will show how each account can get its own state within the contract.

## Signer Account

All change calls need to be signed by an account, this proves that no other account made the transaction. From the contract's perspective, when executing it has access to the current context which includes the `signer` account id.

Since this account id is unique to the signer it can be used as a key in storage. Let's take a look.

### `env`, `AccountId`, and `LookupMap`

```rust,noplayground,ignore
{{#include ../../examples/contracts/status-message/contracts/simple/src/lib.rs:1:6}}
```

`env` provides functions for interacting with the NEAR blockchain. This includes `env::signer_account_id()`, which as you expect returns the account id of the signer.

`AccountId` is a wrapper type around `String`. Using it ensures that you won't use a malformed account id.

`LookupMap` is our first experience with a persistent data structure. You can think of them like corresponding data structures that have their data in storage. In this case `LookupMap` is like a `HashMap`, but the keys are contract storage keys and values are written to storage.
