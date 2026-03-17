# Per-Particle Box Registers (Avoiding Quantum Addressing)

> **Tags:** #trick #data-structure #QRAM-avoidance #FMM
> **Source:** arXiv:2510.07380 (Quantum FMM, Berry, Wan, Baczewski, Tikku, Babbush), Section IV.1

## What it does
Stores hierarchical/spatial metadata **per particle** instead of per box, so all register accesses happen at fixed locations regardless of quantum state.

## The trick
Classical FMM maintains one data structure per spatial box. Quantumly, which box a particle belongs to is in superposition → accessing "the right box" requires QRAM.

Instead: give each particle $j$ its own registers $Q(j, \ell)$ for every tree level $\ell$. Information is **replicated** — all particles in the same box have identical $Q$ values. But every access is to register $j$ (classically known), not to "the box containing particle $j$" (quantum-determined).

Trade-off: $O(\eta \cdot L)$ registers instead of $O(n_b \cdot L)$, but eliminates all quantum addressing. This same principle motivates the [[Quantum Sorting for Fixed-Location Data Access]] approach.

## When to reach for it
- Quantum algorithms with state-dependent data structures (trees, grids, hash tables)
- When QRAM is unavailable or its overhead destroys the speedup
- Hierarchical algorithms where the tree structure depends on input data

## Complexity
Extra register cost: $O(\eta L)$ value registers where $L$ = tree depth, hence $O(\eta \log N)$ qubits up to the precision of each stored charge/multipole word. Eliminates the implicit $O(\eta)$ overhead from QRAM-style state-dependent addressing at each access.

## Caveat
Information replication means updates must propagate to all copies — the forward-backward sweep handles this, but adds $O(\eta)$ work per level.
