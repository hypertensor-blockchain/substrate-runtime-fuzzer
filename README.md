# Substrate Runtime Fuzzer

`substrate-runtime-fuzzer` is a [substrate](https://github.com/paritytech/polkadot-sdk/tree/master/substrate) runtime fuzzing harness continuously developed at [SRLabs](https://srlabs.de) since 2020. 

It has been used as a part of our continuous audit process for [Parity](https://parity.io) as well as many parachains in the Polkadot and Kusama ecosystem.

As a part of the [Substrate Builders Program](https://substrate.io/ecosystem/substrate-builders-program/), this fuzzer has been used on over 30 different projects, finding dozens of critical vulnerabilities in the process.

## How do I use it?

Here are the steps to launch `substrate-runtime-fuzzer` against the [solochain template runtime](https://github.com/paritytech/polkadot-sdk/tree/master/templates/solochain/runtime):

```
cargo install ziggy cargo-afl honggfuzz grcov
rustup target add wasm32-unknown-unknown
git clone https://github.com/srlabs/substrate-runtime-fuzzer
cd substrate-runtime-fuzzer/templates/solochain/
SKIP_WASM_BUILD=1 cargo ziggy fuzz
```

You can use the the `-j n` flag to run $n$ jobs in parallel.
We also recommend using `--no-honggfuzz` if running for a long time with many cores, as honggfuzz will have a tendency to slow things down and bloat the corpus in this situation.

`SKIP_WASM_BUILD=1` will reduce compilation time and the binary's size. We are not fuzzing the wasm runtime.

### What is "ziggy"?

[`ziggy`](https://github.com/srlabs/ziggy/) is a fuzzer management tool written by the team at SRLabs.

It will spawn multiple different coverage-guided fuzzers with the right configuration and regularly minimize the corpus and share it between instances.

## How do I use this on my substrate-based runtime?

The first step is creating a project and adding the same dependencies as [this project](./templates/solochain/Cargo.toml).
Then, you can modify the `solochain-template-runtime` dependency to your own runtime (either as a local `path` or as a git URL).

Finally you should add a genesis configuration to make sure the fuzzer can reach as much of the code as possible.
You can take inspiration from the genesis configuration of the [kitchensink fuzzer](./templates/kitchensink/src/main.rs).

## License

Substrate Runtime Fuzzer is primarily distributed under the terms of both the MIT license and the Apache License (Version 2.0).

See [LICENSE-APACHE](LICENSE-APACHE) and [LICENSE-MIT](LICENSE-MIT) for details.
