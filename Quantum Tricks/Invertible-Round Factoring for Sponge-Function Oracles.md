# Invertible-Round Factoring for Sponge-Function Oracles

> **Source:** Amy, Di Matteo, Gheorghiu, Mosca, Parent, Schanck, arXiv:1603.09383
> **Tags:** #trick #reversible-computing #SHA3 #Keccak #oracle-construction

## What it does

Builds a reversible oracle for an invertible round function using one temporary state register instead of storing a Bennett history for every round.

## The trick

Suppose a classical permutation round factors into invertible components
$$
R_i = F_m\circ F_{m-1}\circ \cdots \circ F_1.
$$
If some components are cheap linear maps or wire relabellings, and the hard non-linear component is still invertible, compute intermediate values into a temporary register and clear them with inverse components rather than retaining all round states.

For Keccak-p$[1600,24]$, the round is
$$
R_i=\iota_i\circ\chi\circ\pi\circ\rho\circ\theta.
$$
Amy et al. use a 1600-bit temporary register and exploit that $\theta$, $\chi$, and their inverses are available, while $\rho$ and $\pi$ are wire relabellings. Schematically:

```text
input A ── θ ── ρ,π ── χ ── ι_i ── R_i(A)
     │                         │
     └────── inverse cleanup ──┘
```

This avoids storing a fresh 1600-bit snapshot for each of the 24 rounds.

## When to reach for it

- Reversible implementations of sponge functions and permutation-based hashes.
- Block ciphers or permutations whose round functions are made of explicit invertible layers.
- Settings where Bennett history storage is too expensive and the inverse layer formulas are cheap enough.

## Complexity

For SHA3-256 in the source paper, the reversible Keccak-p$[1600,24]$ circuit uses 3200 logical qubits: the 1600-bit state plus one 1600-bit temporary register. The optimised Clifford+T circuit has T-count $499{,}200$ and T-depth $432$.

## Caveat

The trick depends on having efficient inverse circuits. If the inverse of a “simple” classical layer has high reversible cost, the saving over Bennett-style storage can disappear. It also trades space for extra gate work in the inverse cleanup.

## Related notes

- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Checkpointed Key Schedule for Reversible Block-Cipher Oracles]]
- [[Block-Cipher Key Search as a Reversible Grover Predicate]]
