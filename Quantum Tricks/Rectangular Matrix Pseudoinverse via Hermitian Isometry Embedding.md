# Rectangular Matrix Pseudoinverse via Hermitian Isometry Embedding

> **Source:** Wiebe, Braun, Lloyd, arXiv:1204.5242 (2012)
> **Tags:** #trick #linear-algebra #HHL #least-squares #pseudoinverse #isometry

## What it does
Converts a rectangular (non-square) matrix $F$ into a Hermitian operator, making HHL-style quantum linear-system algorithms applicable to least-squares fitting without modification.

## The trick

Given an $N \times M$ matrix $F$ (not necessarily square or Hermitian), define the isometry embedding

$$I(F) = \begin{pmatrix} 0 & F \\ F^\dagger & 0 \end{pmatrix} \in \mathbb{C}^{(N+M)\times(N+M)}.$$

$I(F)$ is Hermitian by construction. Its square is block-diagonal:

$$I(F)^2 = A = \begin{pmatrix} F^\dagger F & 0 \\ 0 & F F^\dagger \end{pmatrix},$$

so $A^{-1}$ contains $(F^\dagger F)^{-1}$ in the upper-left block.

The Moore–Penrose pseudoinverse $F^+ = (F^\dagger F)^{-1} F^\dagger$ is implemented as the two-step sequence

$$|\lambda\rangle \propto A^{-1} \cdot I(F^\dagger) |\mathbf{y}\rangle,$$

where:
- $I(F^\dagger) |\mathbf{y}\rangle$ multiplies by eigenvalues of $I(F)$ (achievable via phase estimation).
- $A^{-1}$ inverts the Hermitian operator $A$ using HHL.

## When to reach for it

- You have a least-squares problem $\min \|F\boldsymbol{\lambda} - \mathbf{y}\|^2$ with non-square $F$ and want to use quantum linear-systems machinery.
- More generally: any time you need to apply a non-Hermitian operator quantum-mechanically; embed it in a larger Hermitian system.
- Applies to quantum regression, quantum PCA setups, and any problem reducible to pseudoinverse computation.

## Complexity

The isometry itself is free (zero overhead). The cost is:
- One call to [[Eigenvalue Multiplication via Phase Estimation]] for the $I(F^\dagger)$ step: $\tilde{O}(\kappa/\varepsilon)$ simulations of $e^{iI(F)t}$.
- One call to HHL / [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] for the $A^{-1}$ step.
- Total: $\tilde{O}(\log(N)\, s^3 \kappa^6 / \varepsilon)$ oracle queries (Childs–Kothari simulation).

## Caveat

- The $\kappa^6$ dependence comes from two separate applications of HHL-style inversion/multiplication. For well-conditioned $F$ this is fine; for ill-conditioned systems it's expensive.
- Modern [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] subsumes this: $F^+$ is a polynomial transform of the singular values of $F$, directly implementable via quantum singular value transformation without the two-step isometry dance.

## Related notes
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Eigenvalue Multiplication via Phase Estimation]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
