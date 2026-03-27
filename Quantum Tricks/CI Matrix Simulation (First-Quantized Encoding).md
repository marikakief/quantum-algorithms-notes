> **Source:** Babbush, Berry, Sanders, Kivlichan, Scherer, Wei, Love, Aspuru-Guzik, arXiv:1506.01029 (2018)
> **Tags:** #trick #quantum-simulation #first-quantized #CI-matrix #fermionic #qubit-efficiency

## What it does

Represents an $\eta$-electron wavefunction using $\tilde{O}(\eta)$ qubits (vs. $\tilde{O}(N)$ for second-quantized), by storing a sorted list of occupied orbital indices rather than a full occupation-number vector. Enables quantum simulation algorithms with better qubit scaling when $\eta \ll N$.

## The trick

**Second-quantized representation:** Store occupation numbers $n_1, n_2, \ldots, n_N \in \{0,1\}$. Requires exactly $N$ qubits. A molecular Hamiltonian has $O(N^4)$ terms.

**First-quantized CI representation:** A Slater determinant is uniquely specified by the set of $\eta$ occupied spin-orbital indices:

$$|\alpha\rangle = |i_1, i_2, \ldots, i_\eta\rangle, \quad i_1 < i_2 < \cdots < i_\eta \in [N]$$

Store each index in $\lceil \log_2 N \rceil$ bits: total register size $\eta \lceil \log_2 N \rceil = \tilde{O}(\eta)$ qubits.

The full CI Hamiltonian in this basis has the following nonzero structure (Slater-Condon rules):
- $H_{\alpha\alpha}$: diagonal element — sum over one-electron terms $h_{ii}$ and two-electron Coulomb/exchange terms for all pairs $i, j \in \alpha$
- $H_{\alpha\beta}$ where $|\alpha \triangle \beta| = 2$ (single excitation $i \to j$): $h_{ij} + \sum_{k \in \alpha \cap \beta}(h_{ikjk} - h_{ikkj})$
- $H_{\alpha\beta}$ where $|\alpha \triangle \beta| = 4$ (double excitation $i,j \to k,l$): $\pm(h_{ijkl} - h_{ijlk})$

The matrix is sparse: each row has at most $O(\eta N + \eta^2 N^2)$ nonzero entries.

**Decomposition:** Interpret each off-diagonal nonzero as a graph edge (Slater determinant pairs that differ by one or two orbitals). Apply edge-coloring to partition the graph into $\Gamma = O(\eta^2 N^2)$ disjoint matchings. Each matching corresponds to a **1-sparse Hermitian matrix** $H_\gamma$ — at most one nonzero per row/column. Each 1-sparse matrix is efficiently simulable and can be made unitary and self-inverse after normalization.

This gives:

$$H = \sum_{\gamma=1}^{\Gamma} H_\gamma, \quad \Gamma = O(\eta^2 N^2)$$

with each $H_\gamma$ 1-sparse. Combine with [[Truncated Taylor Series Simulation]] or other LCU methods to simulate $e^{-iHt}$.

## When to reach for it

- Simulating molecular Hamiltonians when $\eta \ll N$ (sparse occupancy, e.g., small active space or large basis sets).
- When qubit count is the primary constraint and $\eta$ is small enough that $\eta \log N \ll N$.
- As a preprocessing step for any LCU-based simulation algorithm on molecular systems.
- When you need on-the-fly integral evaluation rather than classical precomputation.

## Complexity

- **Qubits:** $\eta \lceil \log_2 N \rceil = \tilde{O}(\eta)$, vs. $O(N)$ for second-quantized
- **Number of 1-sparse terms:** $\Gamma = O(\eta^2 N^2)$ — better than $O(N^4)$ Pauli terms in the second-quantized picture
- **Gate complexity (with [[Truncated Taylor Series Simulation]]):** $\tilde{O}(\eta^2 N^3 t)$ total
- **Oracle cost per query:** $\tilde{O}(N)$ gates for on-the-fly integral evaluation (see [[On-the-fly Molecular Integral Evaluation]])
- **Qubit advantage over 2nd quantized:** factor of $N/(\eta \log N)$ — exponential in $\log$ factor, which is modest in practice but grows with $N$

## Caveat

- Benefit requires $\eta \ll N$. For minimal-basis H₂ ($\eta = 2$, $N = 4$), the improvement is negligible.
- Antisymmetry signs must be tracked explicitly during state manipulations; the sorted-index representation does this correctly but requires careful implementation of the sign oracle.
- The number of 1-sparse terms $\Gamma = O(\eta^2 N^2)$ can be large: for $\eta = 10$, $N = 100$, you get $\sim 10^6$ terms — manageable but not trivial.
- Orbital regularity assumptions (exponential decay, bounded derivatives) are required for the on-the-fly integral bounds in Babbush et al.'s formal proof. Gaussian atomic orbitals may need additional analysis.
- This is a theoretical circuit construction; no hardware demonstration exists for molecules beyond trivial size.

## Related notes
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]]
- [[Truncated Taylor Series Simulation]]
- [[On-the-fly Molecular Integral Evaluation]]
- [[1-Sparse Hamiltonian Decomposition via Graph Coloring]]
- [[Bravyi-Kitaev Transformation]] — second-quantized alternative
- [[Trotterized Time Evolution for Chemistry]] — alternative simulation method
- [[First-Quantized Plane-Wave Chemistry Encoding]] — the other first-quantized approach; $\eta$ registers of $\log N$ qubits in momentum space; supersedes CI encoding for large $N$ ($\tilde{O}(\eta^{8/3} N^{1/3})$ vs $\tilde{O}(\eta^2 N^3)$)
- [[Real-Space Grid Encoding for Many-Body Simulation]] — real-space variant of first quantization using position-bin registers instead of orbital indices
