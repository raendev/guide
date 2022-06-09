## Using RAEN

Finally we can get to some action. 

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

## Deploying

```bash
near dev-delpoy --wasmFile ./target/res/counter_contract.wasm
```

This will create a new throwaway NEAR account on testnet and deploy the contract to that address.

Copy this address and head over to https://raen.dev/admin to interact with the contract.

Like a good cooking show, the contract has already be deployed to [`counter.raendev.testnet`](https://raen.dev/admin/#/counter.raendev.testnet).
