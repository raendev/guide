## Using RAEN

Now we can finally use RAEN to build our contract!

<!-- Add instructions for cloning the examples repo; -->

From within the `rust-contracts` directory use the build command:

```bash
raen build --release
```

or with `npm` script:

```bash
yarn build
```

or

```bash
npm run build
```

This creates a new [Wasm](https://webassembly.org/) binary file in `./target/res/counter_contract.wasm`. This Wasm file has a [Custom Section](https://webassembly.github.io/spec/core/appendix/custom.html) that explains its interfaces in a way other tooling can use. And the best part: even with this extra information embedded in the Wasm binary, the final result still comes out smaller than contracts built without RAEN, thanks to smart compression. (If you're not used to thinking about the compiled size of server-side code, when working with blockchains, you need to [think about it](https://docs.near.org/docs/concepts/storage-staking).)

## Deploying

```bash
near dev-deploy --wasmFile ./target/res/counter_contract.wasm
```

This will create a new throwaway NEAR account on testnet and deploy the contract to that address.

Copy this address and head over to [raen.dev/admin](https://raen.dev/admin) to interact with the contract.

Like a good cooking show, the contract has already be deployed to [`counter.raendev.testnet`](https://raen.dev/admin/#/counter.raendev.testnet).
