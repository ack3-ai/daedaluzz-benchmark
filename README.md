# Daedaluzz L32 benchmark set

A frozen five-maze Solidity benchmark for measuring how EVM fuzzers handle constrained inputs and multi-transaction state.

## What it measures

Each contract stores a position in a 7×7 maze. Four movement functions update that position across transactions, and each accepts eight `uint64` inputs. Non-wall cells contain assertions behind generated arithmetic guards, some satisfiable and some infeasible.

A fuzzer must therefore solve two problems at once:

1. reach the target cell through a valid transaction sequence;
2. supply inputs that satisfy its guard chain.

The score is the number of distinct reachable assertion violations—not line coverage, transaction count, or total assertion hits. Assertion IDs repeat between mazes, so results must be keyed by maze and ID.

## L32 profile

L32 is a constant-magnitude variant of the [Daedaluzz](https://github.com/ConsenSysDiligence/daedaluzz) benchmark generator.

| Parameter | Value |
| --- | ---: |
| Generator seeds | 0–4 |
| Grid | 7×7 |
| Function inputs | 8 × `uint64` |
| Maximum guard constant | 2³²−1 |
| Maximum guard depth | 16 |
| Arithmetic | `unchecked` |

The input width remains 64 bits; L32 changes only the maximum generated guard constant.

## Ground truth

| Maze | Assertion sites | Reachable | Walls |
| ---: | ---: | ---: | ---: |
| 0 | 43 | 36 | 5 |
| 1 | 40 | 24 | 8 |
| 2 | 45 | 32 | 3 |
| 3 | 41 | 27 | 7 |
| 4 | 41 | 34 | 7 |
| **Total** | **210** | **153** | **30** |

Reachability was established by solving every guard with Z3 and replaying each solution against a fresh deployment. A solver model alone does not count: successful EVM replay defines the 153-assertion score ceiling.

## File variants

Each maze is published in the form used by each measured engine:

| Path | Engine | Detection signal |
| --- | --- | --- |
| `mazes/L32/maze-N.sol` | Echidna | `assert(false)` and `AssertionFailed(id)` |
| `mazes/L32/maze-N.wake.sol` | Wake | persistent `found[id]` flag and `-1` return value |
| `mazes/L32/maze-N.foundry.sol` | Foundry | `found[id]` plus Forge `invariant_N` adapters |

The instrumented variants retain the generated guard conditions. On a hit, they record the assertion instead of reverting; movement is committed only when the step returns a non-negative value. The Foundry form imports `forge-std/Test.sol`.

## Reference protocol

The published comparison compiled every contract with solc 0.8.19 and the optimizer set to 200 runs. It used Echidna v2.3.3, Foundry v1.7.1 at `4072e487`, and Wake at `d33810e` with the revm backend.

Coverage campaigns used three seeds per maze and engine: 3 seeds × 5 mazes × 3 engines = 45 runs. Each ran for eight hours on one physical core. Reported curves take medians across seeds and sum the five mazes.

Treat the Solidity files as immutable benchmark inputs. A source, guard, or instrumentation change defines a different benchmark set and should not be reported as L32.

Methodology and results: [Benchmarking EVM Fuzzers Against an Agent-Written Harness](https://ack3.ai/research/benchmarking-evm-fuzzers-against-an-agent-written-harness/).
