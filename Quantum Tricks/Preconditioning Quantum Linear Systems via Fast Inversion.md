
> **Source:** Tong, An, Wiebe & Lin, arXiv:2008.13295, Phys. Rev. A **104**, 032422 (2021)
> **Tags:** #trick #QLSA #preconditioning #block-encoding #QSVT

## What it does

Solves $(A + B)^{-1}|b\rangle$ with cost depending on $\alpha_B$ (the norm of the "hard" part) rather than $\|A + B\|$, by using [[Fast Inversion for Classically-Diagonalizable Matrices|fast inversion]] of $A$ as a preconditioner.

## The trick

Instead of inverting $(A + B)$ directly (condition number $\kappa(A+B)$, potentially huge), rewrite:

$$(A + B)^{-1} = (I + A^{-1}B)^{-1} A^{-1}$$

The preconditioned operator $W = I + A^{-1}B$ has condition number $\kappa(W) \leq (1 + \|A^{-1}\|\|B\|)(1 + \|(A+B)^{-1}\|\|B\|)$, which can be much smaller than $\kappa(A + B)$ when $A$ dominates.

**Implementation:**
1. [[Fast Inversion for Classically-Diagonalizable Matrices|Fast-invert]] $A$ to get a block-encoding of $A^{-1}$
2. Build a [[Block-Encoding Composition Algebra|block-encoding]] of $W = I + A^{-1}B$ by composing the block-encodings of $A^{-1}$ and $B$
3. Invert $W$ via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] polynomial approximation of $1/x$
4. Multiply by the block-encoding of $A^{-1}$
5. Apply to $|b\rangle$ and amplify with cost $O(1/\xi)$ where $\xi = \|(A+B)^{-1}|b\rangle\|$

Total query cost: $O\!\left(\frac{\alpha'_A \alpha_B}{\xi\,\tilde{\sigma}_{\min}} \log\frac{1}{\epsilon}\right)$.

The state-dependent parameter $\xi$ replaces worst-case $1/\kappa$ dependence. For most physical right-hand sides, $\xi \gg 1/\kappa$.

## When to reach for it

- **Green's function computation:** $H = T + V$ where kinetic $T$ is fast-invertible and interaction $V$ is the perturbation. The cost scales with $\alpha_V$ not $\|T\| + \|V\|$.
- **Gibbs state preparation:** combine with contour integral discretisation to implement $e^{-\beta H}$ as an LCU of preconditioned resolvents.
- **Any QLSP where the matrix has a natural splitting** into a large-but-structured piece and a smaller unstructured piece. PDE discretisations (Laplacian + potential) are a natural fit.
- The [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|interaction picture]] exploits a similar $H = A + B$ decomposition for simulation; preconditioning is the linear-algebra analogue of that idea.

## Complexity

Queries to $A^{-1}$ block-encoding: $O(\alpha'_A \alpha_B / \tilde{\sigma}_{\min} \cdot \log(1/\epsilon))$.
Amplification: $O(1/\xi)$ rounds.
The parameters: $\alpha'_A = \|A^{-1}\|$, $\alpha_B$ = block-encoding normalisation of $B$, $\tilde{\sigma}_{\min}$ = lower bound on smallest singular value of $I + A^{-1}B$.

## Caveat

The advantage requires $A$ to be [[Fast Inversion for Classically-Diagonalizable Matrices|fast-invertible]]. If neither piece of $A + B$ has exploitable eigenvalue structure, there's nothing to precondition with.

$\tilde{\sigma}_{\min}$ can degrade when $A^{-1}B$ has eigenvalues near $-1$ — i.e., when $A$ and $B$ nearly cancel. For the Green's function application with frequency $z$ near the spectrum, $\tilde{\sigma}_{\min}$ tracks the spectral gap and the advantage is modest.

The $1/\xi$ lower bound (tight by reduction to search) means that even with perfect preconditioning, state-dependent overhead is unavoidable for hard instances.

## Related notes
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]
- [[Fast Inversion for Classically-Diagonalizable Matrices]]
- [[Inverse-Transform QSVT for Matrix Functions]]
- [[Block-Encoding Composition Algebra]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Interaction-Picture Norm Reduction]]
