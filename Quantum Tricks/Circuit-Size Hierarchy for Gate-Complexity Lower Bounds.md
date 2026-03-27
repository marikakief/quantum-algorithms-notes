# Circuit-Size Hierarchy for Gate-Complexity Lower Bounds

> **Source:** Haah, Hastings, Kothari, Low, arXiv:1801.03922
> **Tags:** #trick #lower-bound #circuit-complexity #Hamiltonian-simulation #counting-argument

## What it does

Proves gate-complexity lower bounds for Hamiltonian simulation by counting how many distinct computations (Boolean functions or distinguishable unitaries) can be produced by circuits of different sizes and locality constraints.

## The trick

The argument has three steps:

**Step 1 — Embed circuits into Hamiltonians.** Any depth-$T$ circuit of geometrically local 2-qubit gates on $n$ qubits can be exactly realised as the time evolution of a piecewise-constant bounded 1D Hamiltonian for time $T$. (Set $H_j = i \log U_j$ for each gate $U_j$.)

**Step 2 — Count what local circuits can do.** Depth-$T$ local circuits on $n$ qubits can compute $2^{\tilde{\Omega}(Tn)}$ distinct Boolean functions. The construction: divide qubits into blocks of $k = \lfloor\log_2 T\rfloor$, implement all $2^{2^{k'}}$ functions on $k' = k - \Theta(\log k)$ bits within each block (this fits in depth $T$), then XOR the outputs across blocks.

**Step 3 — Count what arbitrary circuits can do.** A circuit with $G$ arbitrary (non-local, infinite gate set) 2-qubit gates can compute at most $2^{\tilde{O}(G \log n)}$ distinct Boolean functions. (Each gate is specified by $O(\log n)$ bits for location plus $O(1)$ bits for gate type after Solovay-Kitaev approximation.)

**Conclusion:** To simulate all depth-$T$ local circuits, we need $G \log n = \tilde{\Omega}(Tn)$, giving $G = \tilde{\Omega}(Tn)$.

For unitaries (Theorem 2), replace "distinct Boolean functions" with "distinguishable unitaries" using a packing argument on the unitary group.

## When to reach for it

- Proving gate-complexity (not query-complexity) lower bounds for simulation problems.
- Any setting where the dynamics can embed arbitrary local computations — lattice simulation, circuit simulation, potentially some quantum error correction problems.
- When you need to show that a particular gate-efficient algorithm is essentially optimal.

## Complexity

The lower bound is $\tilde{\Omega}(nT)$ gates for simulating time-$T$ evolution of a bounded 1D local Hamiltonian on $n$ qubits, matching the upper bound from [[Lieb-Robinson Decomposition for Lattice Simulation]] up to polylog factors.

For local measurements only: $\tilde{\Omega}(\min(nT, T^2))$ gates, which is also tight (the light-cone argument gives $\tilde{O}(T^2)$ gates for local observables when $T \leq n$).

## Caveat

- Requires time-dependent (piecewise-constant) Hamiltonians for the lower bound. It's open whether time-independent Hamiltonians on a lattice can be simulated with fewer than $\tilde{\Omega}(nT)$ gates.
- The counting argument is existential — it shows a hard instance exists but doesn't identify a specific one.
- The Solovay-Kitaev step loses polylog factors, so the lower bound is $\tilde{\Omega}$ rather than $\Omega$.

## Related notes
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]]
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
