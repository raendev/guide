# Introduction

First go install `raen`, [instructions here](https://raen.dev/docs/guide/installation.html)

`raen` is a build tool for NEAR smart contracts.

Currently it has a `build` command for compiling your contract, generating it's Application Contract Interface (ACI), and injected into a custom section of a contract's WebAssembly (Wasm) binary.

It plans on having a `fetch` command for pulling down the ACI of deployed contracts to generate the source code bindings needed for cross contract calls and a client interface needed to interact with contracts.

If you have experience with NEAR and want to get to the RAEN specific stuff you can skip to the [status message contract](./status-message/wit.md)
