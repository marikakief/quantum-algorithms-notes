# Block-Diagonal Decomposition for Parallel Hamiltonian Simulation

> **Source:** Abrams & Lloyd, arXiv:quant-ph/9703054 (1997)
> **Tags:** #trick #hamiltonian-simulation #fermions #hubbard-model

## What it does
Decomposes a hopping Hamiltonian into a constant number of block-diagonal pieces, each with small blocks that can all be diagonalised in parallel.

## The trick

For a 1D lattice with nearest-neighbour hopping $T = \sum_i h(i, i+1)$, split into even and odd sublattice terms:

$$T = T_1 + T_2, \quad T_1 = h(1,2) + h(3,4) + \ldots, \quad T_2 = h(2,3) + h(4,5) + \ldots$$

Each $T_k$ is block-diagonal with $2 \times 2$ blocks (sites $\{2i-1, 2i\}$ for $T_1$, sites $\{2i, 2i+1\}$ for $T_2$). All blocks are identical — each is $t_0 \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$.

To diagonalise: map state $|n\rangle \to |(n+1) \text{ div } 2, \; n \bmod 2\rangle$. The first register labels the block, the second is position within the block. Since $T_k$ acts identically on every block, apply a single $2 \times 2$ rotation on the second register — all blocks are diagonalised simultaneously.

## When to reach for it
- Lattice Hamiltonians with translationally invariant local interactions
- Any Hamiltonian where terms can be grouped so that within each group, all terms act on disjoint subsystems
- Parallelising [[Product-Formula Time-Slicing for Local Hamiltonians|Trotter steps]] — commuting terms in the same group can be enacted simultaneously
- Higher-dimensional lattices: colour the interaction graph so each colour class has disjoint edges (graph colouring gives constant many groups for bounded-degree graphs)

## Complexity
$O((\ln m)^2)$ operations per sublattice term (dominated by the quantum-number relabelling arithmetic). The number of sublattice groups is $O(1)$ — independent of system size for local interactions.

## Caveat
Works cleanly for translationally invariant Hamiltonians where all blocks are identical. Non-uniform couplings require per-block phase advances (still parallel, but the rotation angles differ). Higher-dimensional lattices need more colours but the number remains constant for bounded-degree graphs.

## Related notes
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — the split-operator Trotter step uses the same block-diagonal principle at scale: potential-energy terms are diagonal in the dual basis (all commute, apply in parallel), kinetic terms are diagonal in the plane-wave basis; the FFFT alternates between the two diagonal representations
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — fermionic swap network schedules the Hubbard model Trotter step in $O(\sqrt{N})$ depth by exploiting block-diagonal structure of the 2D lattice Hamiltonian (interleaved spin-up/spin-down JW ordering produces near-block-diagonal layers)
