# Daedaluzz L32 benchmark mazes

This repository contains the Solidity maze fixtures used in ack3's [EVM fuzzer benchmark](https://ack3.ai/research/benchmarking-evm-fuzzers-against-an-agent-written-harness/).

The `mazes/L32` set contains five 7×7 mazes generated from fixed seeds with [Daedaluzz](https://github.com/ConsenSysDiligence/daedaluzz). Guard constants are capped at 2³²−1 while function inputs remain `uint64`, and arithmetic is unchecked.

Each maze is published in three forms:

- `maze-N.sol` — the generated contract with failing assertions.
- `maze-N.wake.sol` — an instrumented variant that records reached assertions for Wake.
- `maze-N.foundry.sol` — the same instrumentation with Foundry invariant checks.

The article documents the benchmark methodology, versions, run counts, and results. This repository publishes the Solidity inputs for inspection and reproduction; it is not a standalone benchmark runner.
