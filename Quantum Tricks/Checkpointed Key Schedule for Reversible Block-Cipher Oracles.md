# Checkpointed Key Schedule for Reversible Block-Cipher Oracles

> **Source:** Grassl, Langenberg, Roetteler, Steinwandt, arXiv:1512.04965
> **Tags:** #trick #reversible-computing #resource-estimation #block-ciphers #space-time-tradeoff

## What it does

Reduces qubit storage in a reversible block-cipher oracle by storing expensive key-schedule checkpoints and recomputing cheap XOR-only words when needed.

## The trick

In a classical cipher implementation, the key schedule can be generated forward and overwritten freely. A reversible oracle cannot simply erase intermediate words. The naive option is to store every round key, but this burns qubits.

The AES key schedule has two kinds of words:

1. **Expensive words** involving SubBytes and round constants.
2. **Cheap words** obtainable as XOR combinations of previous words.

Store the expensive words as checkpoints. When a round needs a cheap word, reconstruct it reversibly by CNOTing together the stored checkpoints and nearby words, use it, and then reverse the reconstruction.

In the AES-128 case discussed by the source paper, a word such as $w_{41}$ can be reconstructed by XORing 11 previous words, costing CNOTs and depth but avoiding permanent storage for every expanded-key word.

The same idea also applies to round state: compute several rounds, keep selected irreversible-looking outputs as reversible checkpoints, then reverse part of the computation to free workspace for later rounds.

```text
store:     expensive schedule words  ──────●────────●────────●────
recompute: cheap XOR words on demand        ├─ use ──┤
uncompute: reverse the XOR network          └────────┘
```

## When to reach for it

- Reversible or quantum implementations of block ciphers and hash functions.
- Resource estimates where qubit count is constrained but CNOT overhead is tolerable.
- Oracles with a mix of nonlinear expensive steps and cheap linear update rules.
- Manual circuit designs before applying full reversible pebbling methods.

## Complexity

Let the full key schedule have $W$ words, but only $S$ words require nonlinear expensive computation. Storing only those $S$ words saves roughly $(W-S)$ word registers. The cost is extra reversible linear recomputation whenever a missing word is needed.

For AES in the source paper, this produces the following key-schedule storage costs, excluding the original key:

| Variant | Stored qubits | Ancillae | T gates | Depth |
|---|---:|---:|---:|---:|
| AES-128 | 320 | 96 | 143,360 | 12,636 |
| AES-192 | 256 | 96 | 114,688 | 10,107 |
| AES-256 | 416 | 96 | 186,368 | 16,408 |

The T-count comes from the nonlinear SubBytes calls; the checkpoint/recompute policy mainly trades qubits for CNOTs and depth.

## Caveat

This is a hand-designed checkpointing rule, not an optimal pebbling theorem. It works well when omitted values are cheap linear functions of stored values. If the missing words require nonlinear recomputation, the T-depth and T-count penalty may dominate the qubit savings.

## Related notes

- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Pebble Game Space Reduction for Recursive Circuits]]
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]]
- [[Block-Cipher Key Search as a Reversible Grover Predicate]]
