# Gate Implementation via Graph Scattering

> **Source:** Andrew M. Childs, arXiv:0806.1972
> **Tags:** #trick #quantum-walk #scattering #gate-design #universality

## What it does

Implements quantum gates by designing small graph substructures (widgets) whose scattering matrices at a chosen momentum encode the desired unitary transformation.

## The trick

Represent $n$-qubit computational basis states as $2^n$ parallel semi-infinite lines (wires). Insert a finite graph widget connecting the wires. An incoming plane wave of momentum $k$ on wire $j$ scatters through the widget, producing transmission coefficients $T_{j,j'}(k)$ to each output wire $j'$.

The matrix $T(k)$ at a fixed momentum $k_0$ acts as a quantum gate: if $T(k_0)$ is unitary (no reflection), then the widget perfectly implements the gate $U$ with $U_{j',j} = T_{j,j'}(k_0)$.

**Design procedure:**
1. Pick a target momentum $k_0$ (e.g., $k_0 = -\pi/4$)
2. Design the widget graph so that $|T_{j,j'}(k_0)|^2$ matches the desired gate's matrix elements
3. Verify $R_j(k_0) = 0$ (no reflection) by solving the scattering equations — a system of $|G|$ linear equations for the finite widget $G$
4. Check that the effective lengths $\ell_{j,j'}(k_0) = \frac{d}{dk}\arg T_{j,j'}(k_0)$ are consistent across all channels

**Composition rule:** two widgets in series compose via a geometric series accounting for inter-widget reflections:

$$T_{12} = T_1(1 - R_2\bar{R}_1)^{-1}T_2$$

When both have small reflection ($\|R\| \leq \delta$), the composed transmission satisfies $\|T_{12} - T_1 T_2\| \leq \delta_1\delta_2(1 + \delta_1\delta_2)$.

## When to reach for it

- Designing quantum walk algorithms where the computation is encoded in graph structure rather than explicit gate sequences
- Constructing universality proofs for walk-based computation models
- Building new algorithms based on formula evaluation via scattering (as in the AND-OR formula evaluation work of [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|Ambainis et al. 2007]])

## Complexity

Each widget has $O(1)$ vertices. A universal gate set (CNOT + two single-qubit gates) requires widgets of at most ~10 vertices each. The full graph for an $m$-gate circuit has $O(m \cdot 2^n)$ vertices (exponential in qubits, polynomial in gates).

## Caveat

- Requires momentum filtering to restrict the wave packet to the design momentum $k_0$, adding $\log\Theta(m^2)$ extra filter widgets
- The graph is exponentially large in $n$ (unavoidable since it encodes the full Hilbert space)
- Success probability per run is only $\Omega(1/m^4)$ due to wave packet spreading

## Related notes
- [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]]
- [[Momentum Filtering on Graphs]]
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]
- [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]]
