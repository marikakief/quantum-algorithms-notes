# Fast Inversion for Classically-Diagonalizable Matrices

> **Source:** Tong, An, Wiebe & Lin, arXiv:2008.13295, Phys. Rev. A **104**, 032422 (2021)
> **Tags:** #trick #block-encoding #matrix-inversion #QLSA #preconditioning

## What it does

Block-encodes $A^{-1}$ at cost independent of $\kappa(A)$, when $A$ is diagonalisable by an efficiently-implementable unitary and its eigenvalues are classically computable.

## The trick

For a diagonal matrix $D$ with oracle access to entries $d_j$:

1. Query $d_j$ into an ancilla
2. Compute $1/d_j$ via reversible arithmetic
3. Rotate: $|0\rangle \to \frac{1}{\alpha'_A d_j}|0\rangle + \cdots|1\rangle$ where $\alpha'_A = \|D^{-1}\|$
4. Uncompute; postselect on $|0\rangle$

This gives a $(\alpha'_A, m, 0)$-[[Block-Encoding Composition Algebra|block-encoding]] of $D^{-1}$. Total cost: $O(1)$ oracle queries regardless of $\kappa$.

For **normal** $A = VDV^\dagger$ with efficient $V$ (e.g., QFT, [[Basis Switching via QFT for Kinetic-Potential Splitting|FFFT]]): sandwich the diagonal fast inversion between $V^\dagger$ and $V$.

The key insight: the controlled rotation has success probability $1/(\alpha'_A d_j)^2$ per eigenvalue, but that's absorbed into the block-encoding normalisation $\alpha'_A$ — no $\kappa$-dependent amplification is needed at the block-encoding level.

## When to reach for it

- Any [[Preconditioning Quantum Linear Systems via Fast Inversion|preconditioned QLSP]] where one piece of the operator is "structurally simple" (diagonal in a known basis)
- Kinetic operators in lattice models: Hubbard ($T$ diagonalised by FFFT), plane-wave-dual ($T$ already diagonal), free-fermion hopping (QFT)
- PDE discretisations where the Laplacian is diagonal in Fourier space
- Building block for [[Inverse-Transform QSVT for Matrix Functions|matrix function evaluation]] via preconditioned inversion

## Complexity

$O(1)$ queries to the eigenvalue oracle and $O(1)$ applications of $V, V^\dagger$. Arithmetic circuit for $1/d_j$ costs $O(\text{poly}(\log(1/\epsilon)))$ gates. The normalisation $\alpha'_A = \|A^{-1}\|$ determines downstream amplification costs.

## Caveat

Requires **classically-computable eigenvalues** and an **efficiently-implementable diagonalising unitary**. This fits a specific class of structured matrices — not a general-purpose inversion method. If $A$ has no known diagonalisation, you're back to [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] or [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]]-type approaches.

The arithmetic circuit for eigenvalue inversion adds ancilla qubits and gate overhead that doesn't show up in the query complexity.

## Related notes
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]
- [[Preconditioning Quantum Linear Systems via Fast Inversion]]
- [[Block-Encoding Composition Algebra]]
- [[Basis Switching via QFT for Kinetic-Potential Splitting]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
