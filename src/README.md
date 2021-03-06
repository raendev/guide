# Welcome to RAEN 🌧

RAEN is a build tool for NEAR smart contracts.

With RAEN, you can:

* `build`: compile a contract, generate its Application Contract Interface (ACI), and inject it into a custom section of the contract's [WebAssembly](https://webassembly.org/) (Wasm) binary.
* `fetch` _[coming soon]_: use the ACI of a deployed contract to generate source code bindings for cross contract calls and client interfaces.

NEAR works with any programming language that compiles to Wasm, but the most advanced NEAR SDK and documentation currently exist [for Rust](https://www.near-sdk.io/).

RAEN, too, is built around a [language-agnostic standard](https://github.com/bytecodealliance/wit-bindgen), but currently only works with contracts written in [Rust](https://www.rust-lang.org/).

This guide will introduce you to NEAR, Rust, and RAEN.

### About the name

"RAEN" is "NEAR" spelled backwards. It is pronounced the same as "rain".
