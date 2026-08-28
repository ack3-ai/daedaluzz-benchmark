# Daedaluzz L32 benchmark set

Solidity fixtures prepared for the conference talk *Is Traditional Smart Contract Fuzzing Still Relevant?*, presented at [Web3 Security Summit](https://web3securitysummit.com/) in Belgrade on 26 August 2026.

The set contains five 7×7 mazes generated from seeds 0–4 with [Daedaluzz](https://github.com/ConsenSysDiligence/daedaluzz). The L32 variant keeps `uint64` inputs and caps generated guard constants at 2³²−1.

Each maze is provided in three forms:

- `maze-N.sol` — Echidna
- `maze-N.wake.sol` — Wake
- `maze-N.foundry.sol` — Foundry

Methodology and results: [Benchmarking EVM Fuzzers Against an Agent-Written Harness](https://ack3.ai/research/benchmarking-evm-fuzzers-against-an-agent-written-harness/).
