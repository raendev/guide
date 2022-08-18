# Updating the contract

Without [overflow checks](https://doc.rust-lang.org/cargo/reference/profiles.html#overflow-checks) enabled, if `val` is currently `127`, the next time it is incremented it will _overflow_, wrapping around to be `-128`. Likewise, it can _underflow_ if currently `-128` and you decrement.

This would obviously be **very bad news** if you were building your own smart-contract-based currency!

Let's see if we can make `val` overflow. And then let's see how to prevent this.

## Method 1: mash `increment` 128 times

With the existing contract, you could use the RAEN Admin panel and mash the Submit button on the `increment` form 128 times.

This could take a while, though! Let's see if we can improve the code to make it easier.

## Method 2: new `default`

If you're following along with your own contract, you can change the default value of `val` to start at `127` rather than `0`. This is a good way to learn about where the default of `0` currently comes from.

If you get stuck while following along, you can see this method and the following all implemented on [this branch](https://github.com/raendev/examples/pull/4/files).

Above `pub struct Counter`, change the `derive` macro by removing the `Default` trait:

```diff
-#[derive(Default, BorshDeserialize, BorshSerialize)]
+#[derive(BorshDeserialize, BorshSerialize)]
```

Now, below the closing bracket of `pub struct Counter`, you can implement `default` manually:

```rust
pub struct Counter {
    val: i8,
}

impl Default for Counter {
    fn default() -> Self {
        Self { val: 127 }
    }
}
```

### If you're following along

Build and re-deploy to see the new default:

```bash
near dev-deploy $(raen build --release -q)
```

Refresh your browser page to load in the updated contract. Check `get_num`.

But oh no, if you've already incremented or decremented the number, you'll see that this made no change. Which is what you'd expect! It only changes the _default_. That is, the value that is shown if you didn't actually change it and store the new value.

To see this default in action, you can delete the `neardev` folder and run the `dev-deploy` command again. This will create a brand new NEAR testnet account before deploying your contract. You'll need to copy the new `dev-1234-1234` address it gives you and paste it into RAEN Admin.

Now when you check `get_num`, you'll see the number you set as a default! Call `increment` (you'll need to sign in again, since this is a new contract) and watch it overflow.

### If you're not

The contract deployed at [counter.raendev.testnet](https://raen.dev/admin/#/counter.raendev.testnet) already has a value for `val` saved to contract storage. It will not make use of a new default.

Let's try a different way.

## Method 3: new `set_num`

See if you can write your own `set_num` method based on the code for `increment` and `decrement` in the contract. If you need a hint, [here's what it looks like](https://github.com/raendev/examples/pull/4/files).

### If you're following along

Save your file with new `set_num`, then build and re-deploy:

```bash
near dev-deploy $(raen build --release -q)
```

Unlike Method 2, you don't need to create a new testnet account. Just refresh the RAEN Admin panel and see the `set_num` method appear under Change Methods. Go ahead and try it out!

### If you're not

The [reference code](https://github.com/raendev/examples/pull/4/files) has been deployed to [v2.counter.raendev.testnet](https://raen.dev/admin/#/v2.counter.raendev.testnet) for you to play with.

Note that this code already fixed the overflow issue. If you want to see the overflow in action, you'll have to follow along with your own code.

## Fixing that overflow

Now you can call `increment` and see that the number goes from `127` to `-128`. Oops! How do we fix it?

Open up the `Cargo.toml` file. [Cargo.toml](https://doc.rust-lang.org/cargo/guide/cargo-toml-vs-cargo-lock.html) is to Rust what `package.json` is to NodeJS. You'll see sections for `dependencies` and `dev-dependencies` and more generic information about this package/contract/crate.

See those lines with `overflow-checks = false`? Change them to `overflow-checks = true`. Now rebuild and redeploy:

```bash
near dev-deploy $(raen build --release -q)
```

You'll see that this takes longer to build than when you make changes to your contract itself. Rust needs to recompile all of your dependencies using the new setting.

Refresh your admin panel, set the number back to `127`, and call `increment`. You'll see an error message. This means the contract panicked rather than allowing the integer overflow. It worked!

## Digging Deeper: wait, aren't blockchains immutable?

You may have heard that blockchains are _immutable_, that they provide permanent ledgers of all on-chain activity.

How does this fit with the fact that you just upgraded a smart contract, and it was actually super easy? It's even called a _contract_—doesn't that imply that it should be hard to change? If it's so easy for a contract owner to alter the contract, how can anyone using it trust they won't be scammed?

Great questions!

On other blockchains, it is in fact very difficult to upgrade a contract. They work around this with proxy contracts and other patterns.

But NEAR has that handy account system, where each account can have multiple keys. We already learned about _Function Call Access Keys_ last unit; that's what makes it safe to have a web-based wallet, with web apps that store a restricted private key in a browser's local storage.

The other kind of key is a _Full Access_ key. When you run `dev-deploy`, you get a Full Access key to a brand new testnet account. If you have a Full Access key, you can do anything with that account. Deploy or remove a contract, call other contracts, send tokens, create sub-accounts, even delete the account.

(Note that even all of these actions are stored on the permanent blockchain ledger! If you're running or know the address of an [archival node](https://near-nodes.io/archival/run-archival-node-without-nearup), you can query the state of a given account at a given block/second in time, even if the contract on that account has changed or the account has been completely deleted. If you're querying a regular non-archival node, it will probably only keep about two days of history.)

In the early days of launching and testing a smart contract, it's nice to be able to leave the contract in "trusted mode," keeping Full Access keys around so you can iterate quickly. But if you want people to trust your contract with large investments, you'll want to get a security review from a reputable company, and those companies will want to make sure you don't have any Full Access keys still on the contract (you can get a list of all keys on any NEAR account with `near keys` in your command line), and that you didn't build in any sneaky ways to add new Full Access keys given only a Function Call key.

(Note that there still patterns that allow you to upgrade such a contract, such as [requiring a review board to vote on proposed upgrades](https://www.near-sdk.io/upgrading/via-dao-vote).)

If you're _using_ a smart contract, especially if you're considering trusting it with significant assets, you should check that it had a security review first!

## Summary

In this unit we learned about integer overflows, and played with them by updating our code to change the default value and add a new change method. We saw that we only need to refresh the RAEN Admin page to see the latest changes. We fixed the integer overflow bug by changing `Cargo.toml` settings. And we also learned about blockchain immutability and Full Access keys.

Next, let's learn how to deploy to a nicely-named account like `counter.raendev.testnet`.
