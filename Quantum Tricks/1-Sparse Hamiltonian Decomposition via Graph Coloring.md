
> **Source:** Babbush, Berry, Sanders, Kivlichan, Scherer, Wei, Love, Aspuru-Guzik, arXiv:1506.01029 (2018); earlier: Toloui & Love (2013)
> **Tags:** #trick #sparse-simulation #graph-coloring #1-sparse #LCU #hamiltonian-decomposition

## What it does

Decomposes a sparse Hermitian matrix $H$ into a sum of 1-sparse Hermitian matrices (each row and column has at most one nonzero off-diagonal entry) by treating the nonzero off-diagonal entries as edges of a graph and edge-coloring the graph. Each 1-sparse term can be made into a self-inverse unitary, enabling efficient LCU-based simulation.

## The trick

**Setup:** Let $H$ be a sparse Hermitian matrix on a Hilbert space with basis $\{|\alpha\rangle\}$. Treat each pair $(\alpha, \beta)$ with $H_{\alpha\beta} \neq 0$, $\alpha \neq \beta$, as an undirected edge in a graph $G$ on vertices $\{|\alpha\rangle\}$.

**Key observation:** If you can properly edge-color $G$ with $c$ colors — so that no two edges sharing a vertex have the same color — then you can write:

$$H = H_\text{diag} + \sum_{\gamma=1}^{c} H_\gamma$$

where each $H_\gamma$ contains only the edges of color $\gamma$. Since no vertex touches two edges of the same color, each $H_\gamma$ is **1-sparse**: each row and column has at most one nonzero off-diagonal entry.

**From 1-sparse to self-inverse unitary:** A 1-sparse Hermitian matrix with entries bounded by $\lambda$ can be written:

$$H_\gamma = \lambda \cdot \sum_{\text{edges } (\alpha, \beta)} \frac{H_{\gamma,\alpha\beta}}{\lambda} (|\alpha\rangle\langle\beta| + |\beta\rangle\langle\alpha|)$$

Normalize and round entries to $\pm 1$ (using binary decomposition with rounding parameter $\zeta$) to obtain a sum of **self-inverse unitaries** with $\pm 1$ entries. This is directly compatible with the SELECT oracle structure needed for [[Truncated Taylor Series Simulation]] or other LCU methods.

**For the CI matrix (Babbush et al.):** The nonzero off-diagonal entries of the CI matrix connect Slater determinants $\alpha, \beta$ that differ by one or two orbital occupations. The graph $G$ has structure governed by which pairs of orbitals are involved in the excitation. A carefully chosen coloring (using an 8-tuple index) achieves:

$$c = \Gamma = O(\eta^2 N^2)$$

colors for $\eta$ electrons in $N$ orbitals. (Toloui & Love's earlier coloring used $O(N^4)$ colors; Babbush et al. improve this to $O(\eta^2 N^2)$ by exploiting the restriction to $\eta$-electron sectors.)

**Color oracle:** The coloring must be efficiently computable: given $\alpha$ and color $\gamma$, output the unique $\beta$ such that $(\alpha, \beta)$ is an edge of color $\gamma$ (or indicate that $\alpha$ is isolated in this color). This is the $Q_\text{col}$ oracle in [[CI Matrix Simulation (First-Quantized Encoding)]].

## When to reach for it

- Whenever you have a sparse Hermitian matrix and need to decompose it into terms that are efficiently simulable (1-sparse, or more generally, few-body operators).
- As the preprocessing step for any LCU-based simulation (Taylor series, QSVT, quantum walk) applied to sparse Hamiltonians.
- For molecular Hamiltonians (CI or second-quantized) where structure in the sparsity pattern allows better-than-generic colorings.
- For lattice models, tight-binding Hamiltonians, or any Hamiltonian whose nonzero structure forms a graph with low chromatic index.

## Complexity

- **Number of colors:** $c = O(\Delta)$ where $\Delta$ is the maximum degree of $G$ (by Vizing's theorem, the chromatic index is $\Delta$ or $\Delta+1$).
  - For the CI matrix with $\eta$ electrons, $N$ orbitals: $c = O(\eta^2 N^2)$
  - For a generic $d$-sparse matrix on $n$ states: $c = O(d)$
- **Color oracle cost:** $\tilde{O}(1)$ per query (just index arithmetic on $\eta \log N$ bits) for the CI case
- **Simulation overhead:** $c$ extra terms in the LCU decomposition, directly increasing gate count by a factor of $c$
- **Each 1-sparse simulation:** $O(1)$ calls to the value oracle + $O(1)$ state swaps

## Caveat

- The number of colors $c$ determines the overhead. Choosing a good coloring is problem-specific. For CI: $O(\eta^2 N^2)$. For second-quantized: $O(N^4)$ Pauli terms. The CI coloring is better when $\eta \ll N$.
- Computing the coloring must be efficient (both classically for circuit design and as an in-circuit oracle). Not all graph colorings have efficient oracular descriptions.
- The binary rounding step (to get $\pm 1$ entries) introduces a rounding error $\zeta$ that must be budgeted against the total allowed simulation error $\varepsilon$.
- Diagonal terms ($H_\text{diag}$) need separate treatment: they are not 1-sparse in the same sense, though they commute with the computational basis and can be handled via a phase oracle.

## Related notes
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]]
- [[CI Matrix Simulation (First-Quantized Encoding)]]
- [[Truncated Taylor Series Simulation]]
- [[On-the-fly Molecular Integral Evaluation]]
- [[Trotterized Time Evolution for Chemistry]] — alternative decomposition strategy
