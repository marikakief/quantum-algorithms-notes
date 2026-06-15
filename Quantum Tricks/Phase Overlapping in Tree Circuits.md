# Phase Overlapping in Tree Circuits

> **Source:** Draper, Kutin, Rains, Svore, arXiv:quant-ph/0406142
> **Tags:** #trick #circuit-optimisation #depth-optimised #scheduling #quantum-arithmetic

## What it does

Reduces the depth of a multi-phase tree computation by running independent phases in parallel when they operate on disjoint data.

## The trick

The [[Carry-Lookahead Tree for Logarithmic-Depth Addition|carry-lookahead tree]] has four sequential phases: P-rounds (build propagate tree), G-rounds (build generate tree), C-rounds (propagate carries down), and P$^{-1}$-rounds (erase propagate tree). Naively, this would take $\sim 4\lfloor \log n \rfloor$ depth.

The key observation is data independence:

- **P-round $t+1$ and G-round $t$** operate on disjoint registers: P-round $t+1$ reads $P_{t}$ values; G-round $t$ reads $G$ and $P_{t-1}$ values. Since $P_t$ and $P_{t-1}$ are different registers, these rounds can run simultaneously.
- **C-round $t-1$ and P$^{-1}$-round $t$** are similarly independent: the C-round reads $P_{t-2}$ and $G$, while the P$^{-1}$-round erases $P_t$ using $P_{t-1}$.

This compresses the schedule from $4\lfloor \log n \rfloor$ to roughly $\lfloor \log n \rfloor + \lfloor \log(n)/3 \rfloor + O(1)$ — close to a $3\times$ improvement in depth for the carry computation phase.

| Logical phase | Level-$t$ work uses | Can overlap with |
|---|---|---|
| P-round $t+1$ | $P_t$ values to build the next propagate level | G-round $t$, which reads $G$ and $P_{t-1}$ |
| G-round $t$ | generate values and the previous propagate level $P_{t-1}$ | P-round $t+1$ |
| C-round $t-1$ | carries/generates and lower propagate level $P_{t-2}$ | P$^{-1}$-round $t$ |
| P$^{-1}$-round $t$ | $P_t$ and $P_{t-1}$ values to erase propagate scratch | C-round $t-1$ |

## When to reach for it

- Any tree-structured quantum computation where the up-sweep (compute) and down-sweep (propagate or erase) phases touch different levels of the tree at each step
- Parallel prefix circuits (carry-lookahead, Brent-Kung, Kogge-Stone) adapted to the quantum setting
- More generally, any multi-pass computation where later passes at one level depend on earlier passes at a *different* level, creating opportunities for pipelining

## Complexity

No additional gates or ancillae. Pure scheduling optimisation. The depth saving is from $4\lfloor \log n \rfloor$ to $\lfloor \log n \rfloor + \lfloor \log(n)/3 \rfloor + O(1)$ in the QCLA case.

## Caveat

Requires the hardware to support parallel gate execution on disjoint qubits within the same time step. On architectures with limited parallelism (e.g., linear nearest-neighbour with routing overhead), the depth reduction may not translate directly to wall-clock savings. The overlapping also makes the circuit harder to reason about — each time step contains gates from two logically distinct phases.

## Related notes

- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]]
- [[Carry-Lookahead Tree for Logarithmic-Depth Addition]]
- [[Two's-Complement Carry Erasure for In-Place Adders]]
- [[Gate Commutation for Ripple-Carry Depth Reduction]] — a different depth-reduction technique for ripple-carry adders (commuting adjacent gates within the same linear chain)
