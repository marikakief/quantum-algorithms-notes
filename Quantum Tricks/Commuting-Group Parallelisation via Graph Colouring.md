# Commuting-Group Parallelisation via Graph Colouring

> **Source:** Raeisi, Wiebe & Sanders, arXiv:1108.4318 (2012)
> **Tags:** #trick #hamiltonian-simulation #parallelization #product-formula #circuit-depth

## What it does

Reduces the circuit depth of a product-formula simulation by grouping mutually commuting Hamiltonian terms and executing each group in parallel, turning an $O(M)$-depth slice into an $O(P)$-depth slice, where $P$ is the number of commuting colour classes (often $P \ll M$).

## The trick

1. **Build the non-commutativity graph.** Vertices = Hamiltonian terms $\{h_j\}$; edge $(i, j)$ exists iff $h_i$ and $h_j$ do **not** commute. For Pauli strings: $h_i, h_j$ anticommute iff

$$\sum_{v \neq w} |S^v_i \cap S^w_j| \equiv 1 \pmod{2}$$

(count positions where one acts as $X$ or $Y$ and the other as $Y$ or $Z$ in a crossing pattern; odd count = anticommute).

2. **Colour the graph.** A proper graph colouring assigns each term a colour such that adjacent (non-commuting) terms get different colours. Each colour class is a set of mutually commuting terms.

3. **Within each colour class:** all terms commute, so their exponentials (as [[Pauli-String Exponential via Parity-CNOT Ladder|Pauli-string primitives]]) can be executed in **any order** and their CNOT ladders act on disjoint qubit sets (for physically local Hamiltonians with bounded degree). Schedule them in parallel.

4. **Between colour classes:** the product-formula ordering across classes must be respected (non-commuting terms are in different classes for exactly this reason).

**Depth reduction:** without grouping, a product-formula slice of $M = 2m(5/3)^{\chi-1}$ exponentials has depth $O(k \cdot M)$ (each primitive takes $O(k)$ gates in sequence). With grouping into $P$ colour classes, depth drops to $O(k \cdot P)$. For physically local Hamiltonians (bounded-degree interaction graph), $P = O(1)$ by the [[Block-Diagonal Decomposition for Parallel Hamiltonian Simulation|graph colouring of bounded-degree graphs]].

## When to reach for it

- Any product-formula circuit for a local Hamiltonian (lattice, molecular, hardware-native interaction graph).
- VQE or QAOA layers where multiple commuting rotation generators can fire simultaneously.
- Whenever the interaction graph is sparse (degree $d$): greedy colouring gives $\leq d+1$ colours, so depth improvement is $\sim M/(d+1)$.

## Complexity

- **Greedy colouring:** $O(m^2)$ classical preprocessing (check each pair for commutativity; assign first valid colour). Optimal colouring is NP-hard in general, but greedy gives $\leq \Delta + 1$ colours for maximum degree $\Delta$.
- **Circuit depth per slice:** $O(k \cdot P)$ where $P$ is the number of colour classes ($P \leq \Delta + 1$ for bounded-degree graphs; $P = O(1)$ for constant-degree local models).
- **Total circuit depth:** $O(k \cdot P \cdot r)$ vs $O(k \cdot M \cdot r)$ ungrouped; improvement factor $M/P$.

## Caveat

- Optimal colouring (minimum $P$) is NP-hard; the greedy approximation may not be tight.
- For dense Hamiltonians (e.g., fully connected pairing models), the interaction graph has degree $\Theta(n)$ and colouring gives $\Theta(n)$ classes — depth is not reduced below the sequential $O(n)$ per slice.
- Parallelism only helps if the hardware can actually execute the parallel primitives simultaneously (connectivity and gate parallelism on the target device must support it).
- This is purely a depth/time optimisation; it does not reduce gate count.

## Related notes
- [[Quantum-Circuit Design for Efficient Simulations of Many-Body Quantum Dynamics (Raeisi-Wiebe-Sanders 2012) — Paper Notes]]
- [[Pauli-String Exponential via Parity-CNOT Ladder]]
- [[Block-Diagonal Decomposition for Parallel Hamiltonian Simulation]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Suzuki Recursion for Ordered Exponentials]]
