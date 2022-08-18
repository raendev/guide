# Deploying to a subaccount

You might be wondering how to make a _named_ contract, like `counter.raendev.testnet` instead of `dev-1234-1234`. Some news for you:

## They're all named contracts!

If you're used to other blockchains, then you're familiar with the idea of a public key being synonymous with an account. Accounts and public/private keypairs are equivalent in blockchains like Ethereum and Bitcoin, but not NEAR!

NEAR accounts _can_ copy this "public key as account name" setup. These are called [implicit accounts](https://docs.near.org/integrator/implicit-accounts). A neat thing about them is that you can create a unique, valid account address _before making any on-chain transactions_, entirely locally, using the magic of cryptography.

But most NEAR accounts you'll interact with are named. Like your account, or `raendev.testnet`. Furthermore:

- Any account can have subaccounts, like `counter.raendev.testnet`
- Any account can have a contract deployed to it

Your `dev-1234-1234` contract isn't an implicit account. It's a named account, with a contract deployed to it. It just has a bad name! How can you give it a good name?

## CLI Login

In your command line, run the command:

```bash
near login
```

This will open up testnet NEAR Wallet in your browser, and request a _Full Access_ key for your account. Go ahead and say Yes.

Now verify that you have a file in `~/.near-credentials/testnet`:

```bash
cat ~/.near-credentials/testnet/*.testnet.json
```

Great! Now you can use your named account from the command line.

## Create a subaccount

You could deploy the counter contract right to the account you just used with `near login`, but you may want to leave it free for a different contract later. Or you might just want to leave it contract-free forever!

Let's create `counter.YOUR-ACCOUNT.testnet`, but with your actual account name instead of `YOUR-ACCOUNT`. To make the following commands copy-pastable, let's set up a bash variable. Note that this is using bash syntax, which only works with bash, zsh, and a couple other unix-type shells. If you're using Windows, you may just want to modify all the commands that follow.

Otherwise, this is the only command that you need to modify:

```bash
export ACCOUNT=your-account.testnet
```

Ok, now you can create a subaccount:

```bash
near create-account counter.$ACCOUNT --masterAccount $ACCOUNT --initialBalance 2
```

This makes a call to the blockchain as `your-account.testnet` (that's the `masterAccount` part), creating a subaccount, `counter.your-account.testnet`. It sends 2 NEAR tokens from `your-account.testnet` to the new account.

You can check that this was created correctly on NEAR Explorer. Paste your correct account name into this URL:

    https://explorer.testnet.near.org/accounts/counter.your-account.testnet

You can also check the `~/.near-credentials/testnet` folder again and see that a JSON file was added for this new one, too:

```bash
cat ~/.near-credentials/testnet/*.testnet.json
```

Subaccounts are a powerful and flexible feature of NEAR. You can create deeply nested subaccounts, such as `v2.counter.your-name.testnet`, and each one is a valid account with the same rights and permissions. An account can only create subaccounts of itself, not of its subaccounts (`counter.your-name.testnet` can create `v2.counter.your-name.testnet`, but `your-name.testnet` cannot).

In fact, `testnet` is an account, and all accounts that end in `.testnet` are subaccounts of it! It has a special contract deployed to it that makes it easy to create subaccounts. The same is true on NEAR's mainnet: [`near` is an account](https://explorer.near.org/accounts/near), like any other. It's just a _top level account_, kind of like a top-level domain on the web (`.com`, `.net`, etc). Someday more top-level accounts will be made available and auctioned off.


## Deploy to your subaccount

Right now, your subaccount has no contract deployed to it. If you run `near state counter.your-name.testnet`, you'll see that `code_hash` is all ones. This means it has no code—no contract—deployed to it:

```bash
near state counter.$ACCOUNT
```

Let's fix that:

```bash
near deploy counter.$ACCOUNT $(raen build --release -q)
```

That's it! Very similar to `dev-deploy`, but you need to tell it which account to deploy to. You'll also need to have credentials stored in `~/.near-credentials` matching the account name.

You can verify that the code hash changed, if you want:

```bash
near state counter.$ACCOUNT
```

And you can go enter `counter.your-account.testnet` into RAEN Admin. It should look the same as the old `dev-1234-1234` contract. But such a better name!

## Summary

Over this whole Counter chapter, we saw our first Rust code: a simple NEAR smart contract that implements a counter. Anyone can call `get_num` to see the current value, and anyone can change the number, too! We learned about Cargo, Rust's package manager, in order to fix a bug. We saw that updating contracts is easy thanks to Full Access keys, and that we only need to refresh RAEN Admin to see the latest contract changes.

In this unit we experienced more of NEAR's account name system, adding a Full Access key to our local machines by using `near login`. This allowed us to create a subaccount, which also added a Full Access key for that new subaccount. And we deployed the Counter contract to that new account.

Overall, this Counter was a good way to learn some Rust and NEAR basics, but pretty silly as a real contract. Next, let's take a look at an example that does something more useful, storing different information for every user who calls the contract.
