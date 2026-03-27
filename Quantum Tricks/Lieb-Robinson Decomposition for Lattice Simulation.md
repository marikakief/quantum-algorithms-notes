# Lieb-Robinson Decomposition for Lattice Simulation

> **Source:** Haah, Hastings, Kothari, Low, arXiv:1801.03922
> **Tags:** #trick #hamiltonian-simulation #lattice #Lieb-Robinson #gate-complexity #geometrically-local

## What it does

Decomposes the time evolution of a geometrically local lattice Hamiltonian into a product of small block unitaries, each acting on $O(\log^D(nT/\epsilon))$ qubits, with total error exponentially small in the overlap size.

## The trick

For a geometrically local Hamiltonian $H = \sum_{X \subseteq \Lambda} h_X$ on a $D$-dimensional lattice of $n$ qubits, information propagates at finite speed $v_{\mathrm{LR}}$ ([[Lieb-Robinson bound]]). This means the time-evolution unitary for short time $t = O(1)$ can be factored by cutting the lattice into overlapping regions:

$$e^{-itH} \approx e^{-itH_A} \cdot e^{+itH_Y} \cdot e^{-itH_{Y \cup B}}$$

where $A \cup B = \Lambda$, $Y = A \cap (Y \cup B)$ is an overlap region of width $\ell$, and $H_A$, $H_Y$, $H_{Y \cup B}$ are the Hamiltonian restricted to those regions. The error is $O(e^{-\mu\ell})$ for a constant $\mu > 0$.

**Recursive application:** apply the same decomposition to each large block until every block has size $O(\ell^D)$ where $\ell = O(\log(nT/\epsilon))$. For $D$ dimensions this takes $D$ rounds, producing $O((L/\ell)^D)$ blocks.

**Block simulation:** each $O(\ell^D)$-qubit block is simulated by any algorithm with polylogarithmic precision dependence — [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]], [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Taylor series]], or [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]].

**Net cost:**

$$O\left(nT \operatorname{polylog}\frac{nT}{\epsilon}\right) \text{ gates}, \qquad O\left(T \operatorname{polylog}\frac{nT}{\epsilon}\right) \text{ depth}$$

All gates are geometrically local 2-qubit gates, and only $O(1)$ ancillas per qubit are needed.

## When to reach for it

- Simulating any lattice Hamiltonian (condensed matter, lattice gauge theories, quantum materials) where qubits sit on a bounded-dimensional grid and interactions are local.
- When you want gate complexity proportional to spacetime volume $nT$ rather than $n^2 T$ (which sparse oracle methods give).
- When circuit depth matters — the depth scales as $O(T)$ times polylogs, matching the physical evolution time.
- Especially useful at scale ($n \gtrsim 100$ for 1D Heisenberg) where the overhead from the decomposition is amortised.

## Complexity

- **Gates:** $O(nT \operatorname{polylog}(nT/\epsilon))$ — quasilinear in spacetime volume
- **Depth:** $O(T \operatorname{polylog}(nT/\epsilon))$
- **Ancillae:** $O(1)$ per system qubit (interspersed in the lattice)
- **Layers per time step:** $2D + 1$ with hyperplane cuts; $2\alpha - 1$ with $\alpha$-colourable tessellation (e.g., 5 layers for hexagonal 2D, 7 for BCC 3D)

## Caveat

Only works for **geometrically local** Hamiltonians on bounded-dimensional lattices. Does not apply to:
- Non-geometrically-local Hamiltonians (e.g., second-quantised chemistry with all-to-all Coulomb terms)
- Lattices with unbounded dimension (the layer count $2D+1$ grows)
- Small systems ($n \lesssim 100$ in 1D) where the decomposition overhead exceeds direct simulation

The crossover point where this beats direct [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] depends on the specific Hamiltonian — for the Heisenberg model, around $n \sim 100$ qubits.

## Related notes
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
- [[Circuit-Size Hierarchy for Gate-Complexity Lower Bounds]]
- [[Block-Diagonal Decomposition for Parallel Hamiltonian Simulation]]
- [[Truncated Taylor Series Simulation]]
