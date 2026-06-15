# Macaulay-HHL Pseudo-Solution States

> **Source:** Chen-Gao, arXiv:1712.06239
> **Tags:** #trick #HHL #polynomial-systems #macaulay-matrix

## What it does

Turns a Macaulay linear system into a quantum state whose amplitudes are a linear combination of monomial evaluations at the polynomial system's solutions.

## The trick

Given a zero-dimensional radical ideal $(F)\subset\mathbb C[X]$ with
$$
V_{\mathbb C}(F)=\{a_1,\ldots,a_w\},
$$
construct the modified Macaulay system
$$
M_{F,D}m_D=b_{F,D}
$$
for $D\ge \operatorname{CSdeg}(F)$. Algebraically, every nonzero-column solution is an affine combination
$$
\sum_{i=1}^w \eta_i m_D(a_i),\qquad \sum_i\eta_i=1,
$$
up to arbitrary components in zero columns.

Run a QLSA/HHL routine on the Macaulay system. HHL returns the minimum-norm solution state. Because the zero-column components are orthogonal to the true monomial-evaluation span, the minimum-norm choice sets them to zero. The output is therefore a pseudo-solution state
$$
\left|\sum_{i=1}^w \eta_i\widetilde m_D(a_i)\right\rangle.
$$

The Macaulay matrix has a 1-sparse decomposition indexed by the terms of the input polynomials, so the sparse-linear-system oracle can be built from term and exponent arithmetic rather than by materialising the huge matrix.

## When to reach for it

Use this when a nonlinear algebraic problem has a low-degree monomial embedding and you only need a sample or witness extracted from a solution state, not all solution coordinates as classical numbers.

Good fits:

- Boolean or finite-alphabet solution extraction.
- Algebraic cryptanalysis framed as sparse polynomial solving.
- Cases where Macaulay matrices are queryable and the relevant condition number is expected to be small.

## Complexity

For total sparseness $T_F$ and complete solving degree bound $D$, Chen-Gao give
$$
\widetilde O\!\left(n\log(D)T_F\kappa^2/\epsilon\right)
$$
for the Macaulay-HHL state preparation, where $\kappa$ is the condition number of $M_{F,D}$.

## Caveat

This does not solve the readout problem by itself. Measuring the state gives a monomial label, not the full solution vector. It is useful only when the downstream problem can interpret such monomial samples, as in [[Boolean Solution Extraction by Monomial Measurement]]. Also, if $\kappa$ is exponential, the claimed speedup disappears.

[[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes|Ding--Gheorghiu--Gilyén--Hallgren--Li (2023)]] make this caveat concrete: for the Chen--Gao max-degree Macaulay construction, the monomial solution vector itself can force a large [[Truncated QLS Condition Number as a Right-Hand-Side No-Go Test|right-hand-side condition number]].

## Related notes

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Complete Solving Degree for Monomial Recovery]]
- [[Boolean Solution Extraction by Monomial Measurement]]
- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]
- [[Hamming-Weight Norm Lower Bounds for Macaulay Solution States]]
