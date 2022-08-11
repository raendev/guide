# Updating the contract

[Earlier](./first_steps.md) we said that if an `i8` is currently `127` and you increment it again, it will _overflow_, becoming `-128`. Likewise, it can _underflow_ if currently `-128` and you decrement.

This would obviously be **very bad news** if you were building your own smart-contract-based currency!

Let's see if we can make `val` overflow. And then let's see how to prevent this.

# Method 1: mash `increment` 128 times

With the existing contract, you could use the RAEN Admin panel and mash the Submit button on the `increment` form 128 times.

This could take a while, though! Let's see if we can improve the code to make it easier.

# Method 2: new `default`

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

## If you're following along

If you're following along with your own code, you can build and re-deploy to see the new default:

```bash
near dev-deploy $(raen build --release -q)
```

(This [uses `bash` interpolation](https://stackoverflow.com/a/23656236/249801) put the output of `raen build` in the correct spot in the `near dev-deploy` command. You can try `raen build --release` and `raen build --release -q` on their own to understand the compound command.)

If you've already incremented or decremented the number, though, you'll see that this made no change. Which is what you'd expect! It only changes the _default_. That is, the value that is shown if you didn't actually change it and store the new value.

To see this default in action, you can delete the `neardev` folder and run the `dev-deploy` command again. This will create a brand new NEAR testnet account before deploying your contract.

Paste this new account name into RAEN Admin, check `get_num`, and see the number you set as a default! Call `increment` (you'll need to sign in again, since this is a new contract.)

## If you're not

The contract deployed at [counter.raendev.testnet](https://raen.dev/admin/#/counter.raendev.testnet) already has a value for `val` saved to contract storage. It will not make use of a new default.

Let's try a different way.

# Method 3: new `set_num`

See if you can write your own `set_num` method based on the code for `increment` and `decrement` in the contract. If you need a hint, [here's what it looks like](https://github.com/raendev/examples/pull/4/files).

## If you're following along

Save your file with new `set_num`, then build and re-deploy:

```bash
near dev-deploy $(raen build --release -q)
```

Unlike Method 2, you don't need to create a new testnet account. Just refresh the RAEN Admin panel and see the `set_num` method appear under Change Methods. Go ahead and try it out!

## If you're not

The [reference code](https://github.com/raendev/examples/pull/4/files) has been deployed to [v2.counter.raendev.testnet](https://raen.dev/admin/#/v2.counter.raendev.testnet) for you to play with.

Note that this code already fixed the overflow issue. If you want to see the overflow in action, you'll have to follow along with your own code.

# Fixing that overflow

Now you can call `increment` and see that the number goes from `127` to `-128`. Oops! How do we fix it?

Open up the `Cargo.toml` file. [Cargo.toml](https://doc.rust-lang.org/cargo/guide/cargo-toml-vs-cargo-lock.html) is to Rust what `package.json` is to NodeJS. You'll see sections for `dependencies` and `dev-dependencies` and more generic information about this package/contract/crate.

See those lines with `overflow-checks = false`? Change them to `overflow-checks = true`. Now rebuild and redeploy:

```bash
near dev-deploy $(raen build --release -q)
```

You'll see that this takes longer to build than when you make changes to your contract itself. Rust needs to recompile all of your dependencies using the new setting.

Refresh your admin panel, set the number back to `127`, and call `increment`. You'll see an error message. This means the contract panicked rather than allowing the integer overflow. It worked!
