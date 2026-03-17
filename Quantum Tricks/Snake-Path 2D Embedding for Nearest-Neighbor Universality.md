
> **Source:** Aharonov, van Dam, Kempe, Landau, Lloyd, Regev, arXiv:quant-ph/0405098, Section 4
> **Tags:** #trick #2-local #nearest-neighbor #embedding

## What it does

Embeds a universal quantum computation into a 2D grid with only 2-local nearest-neighbor interactions, using 6-state particles that carry both clock and computation degrees of freedom.

## The problem

In the basic [[History-State Encoding with Unary Clock|history-state construction]], clock qubits and computation qubits are separate. On a 2D grid, each particle only interacts with 4 neighbors — not enough for a clock qubit to reach the computation qubits it needs to verify.

## The trick

Merge clock and computation into each particle. Each particle has 6 states:

| State | Phase | Role |
|---|---|---|
| $\bigcirc$ | Unborn | Computation hasn't arrived yet |
| $\uparrow$, $\downarrow$ | First phase | Active qubit: $|0\rangle$ or $|1\rangle$ |
| $\Uparrow$, $\Downarrow$ | Second phase | Active qubit after gate application |
| $\times$ | Dead | Computation has moved on |

Arrange $n$ particles in rows, $R+1$ in columns. The computation snakes through:

1. **Downward stage:** Gates apply top-to-bottom in a column, transitioning particles from first to second phase
2. **Upward stage:** Particles "move right" bottom-to-top — second-phase particles die, new first-phase particles appear in the next column

```
 Round 1      Round 2      Round 3
 ↓ col 0→1   ↑ col 1→2   ↓ col 2→3

 ⇕  ○  ○     ×  ↕  ○     ×  ⇕  ○     ×  ×  ↕
 ⇕  ○  ○     ×  ↕  ○     ×  ⇕  ○     ×  ×  ↕
 ⇕  ○  ○     ×  ↕  ○     ×  ⇕  ○     ×  ×  ↕
 ↕  ○  ○     ×  ↕  ○     ×  ↕  ○     ×  ×  ↕
 ↕  ○  ○     ×  ↕  ○     ×  ↕  ○     ×  ×  ↕
 ↕  ○  ○     ↕  ○  ○     ×  ↕  ○     ×  ×  ↕
```

Each step changes at most 2 adjacent particles → 2-local nearest-neighbor.

## Legal shapes verified locally

A shape (assignment of phases to all particles) is legal iff it contains no forbidden nearest-neighbor pair from a list of 10 rules (Table 1 in the paper). This gives a 2-local clock Hamiltonian whose ground space is exactly the legal-shape subspace $S$.

## Why the gap analysis carries over

The restricted Hamiltonians $H''_{S,\ell}$ match the [[Perturbation Lemma for Locality Reduction|3-local versions]] when projected onto $S$. Same [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Markov chain]], same [[Monotonicity Bootstrap for Conductance Bounds|conductance bound]], same $\Omega(1/L^3)$ gap.

## When to reach for it

- Designing physically realistic adiabatic computations with nearest-neighbor interactions
- Any construction where you need local verification of a sequential process on a grid
- Understanding how spatial constraints affect computational universality

## Caveat

Uses 6-state particles (qudits, not qubits). Whether this can be reduced to qubits (2-state) with nearest-neighbor 2-local interactions on a grid is an open question from the paper.

## Related notes

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[History-State Encoding with Unary Clock]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
- [[Monotonicity Bootstrap for Conductance Bounds]]
