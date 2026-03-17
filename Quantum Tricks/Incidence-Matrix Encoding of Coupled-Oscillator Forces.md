# Incidence-Matrix Encoding of Coupled-Oscillator Forces

> **Tags:** #trick #graph-encoding #oscillators #hamiltonian-simulation
> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, *Exponential quantum speedup in simulating coupled classical oscillators*, arXiv:2303.13012, PRX **13**, 041041 (2023)

## What it does
Encodes spring-network interactions so matrix-vector actions correspond directly to edge stretch amplitudes.

## The trick
Build $B$ from graph incidence and stiffness weights, giving sparse access to $BB^\dagger=A$ while preserving physical interpretation. This yields a natural [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of the force matrix.

## When to reach for it
- Graph-structured Laplacian force models

## Complexity
Sparsity of interaction graph directly controls oracle/simulation costs.

## Caveat
General non-graph couplings may lose this structure.

## Related Paper Notes
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]]
