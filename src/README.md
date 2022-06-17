# Welcome to RAEN 🌧

RAEN is a build tool for NEAR smart contracts.

With RAEN, you can:

* `build`: compile a contract, generate its Application Contract Interface (ACI), and inject it into a custom section of the contract's WebAssembly (Wasm) binary.
* `fetch` _[coming soon]_: use the ACI of a deployed contract to generate source code bindings for cross contract calls and client interfaces.

### About the name

"RAEN" is "NEAR" spelled backwards. It is pronounced the same as "rain".

# Getting Set Up

[Install RAEN](https://raen.dev/docs/guide/installation.html), then install the examples repository to follow along:

```bash
git clone --depth 1 --branch v0.0.3 https://github.com/raendev/examples.git --recursive
```

If you have experience with NEAR, you can skip ahead to [Chapter 2: Status Message](./status-message/wit.md).
