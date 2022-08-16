# Getting Set Up

1. **Install or update Rust:**

   ```bash
   curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
   ```

   See [full instructions in the Rust Book](https://doc.rust-lang.org/book/ch01-01-installation.html).

2. **Add the [WebAssembly](https://webassembly.org/) (Wasm) toolchain:** 

   ```bash
   rustup target add wasm32-unknown-unknown
   ```

   NEAR smart contracts need to be compiled to 32-bit Wasm with _architecture_ and _platform_ both "unknown".


3. **Install RAEN:**

   ```bash
   cargo install raen
   ```

   This will install `raen` globally, so it doesn't matter where you run it.

   `cargo` was installed with Rust. Cargo is [the Rust package manager](https://doc.rust-lang.org/cargo/index.html), like NPM for NodeJS. Rust packages are called _crates_. `raen` is a Rust crate.

4. **Install `near-cli`:**

   Prerequisite: [install NodeJS](https://nodejs.dev/learn/how-to-install-nodejs).

   ```bash
   npm install --global near-cli
   ```

   There is a Rust version of near-cli [in the works](https://github.com/near/near-cli-rs), but it's not ready yet. Someday `raen` will wrap `near` (or work with the new Rust-based CLI as a plugin), allowing you to only install one package. But for now, you need both.

5. **Configure your editor:**

   In Visual Studio Code, install the [rust-analyzer extension](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer).

   The tools [recommended by the Rust Book](https://doc.rust-lang.org/book/appendix-04-useful-development-tools.html) will give you further superpowers, making you feel comfy and productive in no time.

6. **Clone the Examples repository:**

   ```bash
   git clone --depth 1 --branch v0.0.4 https://github.com/raendev/examples.git --recursive raen-examples
   ```

   This will clone [github.com/raendev/examples](https://github.com/raendev/examples) to a folder called `raen-examples`.

7. **Build with RAEN:**

   Change into `raen-examples/contracts/counter`, then build:

   ```bash
   raen build --release
   ```

   This wraps `cargo build --release`, adding some extra goodies.

   It may take a minute. Rust fetches dependencies and compiles your project in one step. Subsequent runs will go faster.

   If you skip this, the editor setup from Step 5 won't work. The help docs shown by your editor are fetched along with the dependencies themselves.
