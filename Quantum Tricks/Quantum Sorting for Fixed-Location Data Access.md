# Quantum Sorting for Fixed-Location Data Access

> **Tags:** #trick #data-access #sorting #QRAM-avoidance
> **Source:** arXiv:2510.07380 (Quantum FMM, Berry, Wan, Baczewski, Tikku, Babbush), Section IV

## What it does
Eliminates the need for QRAM by sorting quantum registers so that all subsequent data accesses happen at **classically known register locations**, not quantum-determined addresses. This is the key step that lets the quantum FMM recover $O(\eta \log\eta)$ scaling (matching the classical FMM) instead of paying an extra $O(\eta)$ for quantum addressing overhead.

## The problem being solved
In the FMM, which "box" a particle belongs to depends on its position — which is in quantum superposition. Naïvely, accessing the correct data for each particle's box requires QRAM with $O(\eta)$ address qubits, adding an $O(\eta)$ overhead that negates the FMM speedup entirely.

## The fix
1. Encode positions using **Morton order** (Z-order curve): interleave bits of the $x, y, z$ coordinates. Morton index is directly the binary address in the octree — the top $3\ell$ bits give the level-$\ell$ box label.
2. Apply a **quantum sorting network** to sort all $\eta$ particle position registers by Morton order. After sorting, particles in the same spatial box are contiguous across registers $r_1, r_2, \ldots$ — the register indices of box-members are classically predictable from the sorted order.
3. All subsequent FMM operations (charge accumulation, multipole computation, interaction list traversal) happen at fixed register offsets relative to the sorted position — no runtime quantum addressing.

The paper uses **multiple Morton orderings with shifts** to handle the interaction list: for each pair of boxes in an interaction list, there exists a Morton ordering where the two boxes are at a fixed distance in the sorted sequence, enabling classical-location access.

## When to reach for it
- Any hierarchical spatial algorithm quantized (FMM, Barnes-Hut, tree codes, k-d trees)
- Need to avoid QRAM overhead for position-dependent data
- Particle-based simulations where spatial locality determines interaction structure

## Complexity
Sorting network over $\eta$ registers of $O(\log N)$ bits: standard AKS/odd-even merge networks give $O(\eta \log^2 \eta)$ comparators, each acting on $O(\log N)$-qubit registers (see [[Reversible Comparator with Swap Record]]). Total gate count for sorting: $O(\eta \log^2\eta \cdot \log N)$ gates.

## Caveat
Sorting is a coherent quantum operation — the sorted order is entangled with the particle state. Uncomputation after the FMM step requires running the sorting network in reverse (same cost). The multiple-Morton-ordering strategy requires $O(\log\eta)$ separate sorted orderings (one per tree level), each needing its own sort/unsort pass. Constant-factor overhead is therefore non-trivial.

## Related notes

- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]]
- [[Per-Particle Box Registers (Avoiding Quantum Addressing)]]
- [[Forward-Backward Sweep for Hierarchical Aggregation]]
- [[Shifted Morton Orderings for Interaction List Access]]
- [[Sorting Networks as Quantum Control-Flow Compilers]]
