# Basic Status Message Contract

Previously the contract had state that was shared by any account that can call the contract. Next we will show how each account can get its own state within the contract.

## Signer Account

All change calls need to be signed by an account, this proves that no other account made the transaction. From the contract's perspective, when executing it has access to the current context with includes the `signer` account id.

Since this account id is unique to the signer it can be used as a key in storage. Let's take a look.

### `env` module

