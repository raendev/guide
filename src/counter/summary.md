# Counter Summary

In this chapter we saw our first Rust code, a simple NEAR smart contract that implements a counter. Anyone can call `get_num` to see the current value, and anyone can sign in and call change methods to change the number.

We saw how RAEN Admin makes it easy to interact with contracts built with RAEN. We dug deeper and saw how we can change the contract, re-deploy, and see that RAEN Admin will automatically show the latest version of the contract.

We even investigated some more advanced Rust features, such as changing settings in `Cargo.toml` and implementing a custom `Default` rather than deriving it.

Overall, this Counter was a good way to learn some Rust and NEAR basics, but pretty silly as a real contract. Next, let's take a look at an example that does something more useful, storing different information for every user who calls the contract.
