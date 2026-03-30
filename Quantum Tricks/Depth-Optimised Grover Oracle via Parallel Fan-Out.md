# Depth-Optimised Grover Oracle via Parallel Fan-Out

> **Source:** Campbell, Khurana, Montanaro, arXiv:1810.05582
> **Tags:** #trick #Grover #oracle #depth-optimisation #fan-out #SAT

## What it does

Implements a [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] oracle for checking a conjunction of $m$ local constraints (e.g., $k$-SAT clauses) in Toffoli depth $O(\log k + \log m)$, independent of the number of constraints $m$.

## The trick

The naive approach checks clauses sequentially, giving depth $O(m)$. Instead:

1. **Fan-out** each input bit $x_i$ to $m$ copies using a CNOT tree of depth $\lceil\log m\rceil$. In the surface code, fan-out (CNOT with multiple targets) takes the same time as a single CNOT, so this is essentially free in terms of Toffoli depth.

2. **Check all clauses in parallel:** Each clause is a Toffoli controlled on $k$ bits (with some NOTs for negated literals). Since all clauses are now on separate copies of the input, they can all be checked simultaneously. Toffoli depth: $2(\lceil\log k\rceil - 1) + 1$ using the binary tree decomposition of multi-controlled Toffolis, with Gidney's "uncompute AND" trick replacing half the Toffolis with classically-controlled Cliffords.

3. **AND of all clause results:** A Toffoli controlled on $m$ bits, implemented as a depth-$2(\lceil\log m\rceil - 1)$ binary tree.

4. **Uncompute** all ancillas.

Total Toffoli depth: $4(\lceil\log k\rceil - 1) + 2(\lceil\log m\rceil - 1) + 3$. For the parameters used in the paper ($k=14$, $m\approx 8.86\times 10^5$), this gives depth 53.

For $k = 14$, $m \approx 886{,}000$: Toffoli depth **53**. Compare with Toffoli count $2.4 \times 10^7$ — the parallelism buys enormous depth reduction.

## When to reach for it

- Any Grover-type search where the oracle is a conjunction (AND) of local predicates
- When circuit depth (not width) is the binding constraint — e.g., fault-tolerant settings where T-depth determines runtime
- The trade-off: uses $O(mn)$ qubits for the fan-out copies. Only viable when qubits are cheap relative to time (the surface code regime, where factory qubits dominate anyway)

## Complexity

Toffoli depth $O(\log k + \log m)$. Toffoli count $m(2k + 1) - 3$. Qubit count $O(mn)$ for fan-out copies.

## Caveat

The qubit overhead is large — $O(mn)$ copies of the input. For problems where clauses share many variables, grouping clauses into sets with disjoint variable support and processing groups in parallel (with $mk/n$ copies instead of $m$) reduces the overhead. The depth remains similar.

## Related notes
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
