# Multiplicity Estimation by Restricted-Isotypic Fourier Sampling

> **Source:** Larocca--Havlicek, arXiv:2407.17649
> **Tags:** #trick #quantum-fourier-transform #representation-theory #multiplicity

## What it does

Estimates a branching multiplicity by preparing a $G$-isotypic state, restricting the regular action to $H\subseteq G$, and weak-Fourier-sampling $H$.

## The trick

Suppose $H\subseteq G$ and we want
$$
\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G).
$$
Prepare the maximally mixed $\alpha$-isotypic state in the regular representation of $G$:
$$
\rho_G^\alpha={\Pi_G^\alpha\over d_\alpha^2}
=U_G^\dagger\left(|\alpha\rangle\!\langle\alpha|\otimes {I_{d_\alpha}\over d_\alpha}\otimes {I_{d_\alpha}\over d_\alpha}\right)U_G.
$$
On a control register $\mathbb C[H]$, prepare $|H|^{-1/2}\sum_{h\in H}|h\rangle$, apply controlled left multiplication $R(h)$ on the target, then apply the $H$-QFT and measure only the irrep label.

Character orthogonality gives
$$
\Pr[\beta]
={d_\beta\over d_\alpha}\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G).
$$
Because the multiplicity is an integer, estimating this probability within $d_\beta/(2d_\alpha)$ and rounding recovers the exact answer.

## When to reach for it

Use this when a quantity is naturally a branching multiplicity under subgroup restriction, especially for:

- Kostka numbers via $S_\mu\subseteq S_n$.
- Littlewood--Richardson coefficients via $S_a\times S_b\subseteq S_n$.
- Plethysm coefficients via $S_c\wr S_d\subseteq S_{cd}$.
- Kronecker coefficients via diagonal $S_n\subseteq S_n\times S_n$.

It is also a useful design pattern: turn an algebraic multiplicity into a labelled outcome probability of a Fourier measurement.

## Complexity

If $H$ is the gate cost of the $H$-QFT, $R$ prepares $\rho_G^\alpha$, and $D$ implements the controlled regular action, the cost is
$$
O\!\left((H+R+D)\left({d_\alpha\over d_\beta}\right)^2\right).
$$
The quadratic factor is sample complexity from Chernoff--Hoeffding plus integer rounding.

## Caveat

The method is only efficient when $d_\alpha/d_\beta$ is polynomial and the required QFTs/state preparations are efficient. Many symmetric-group irreps have dimensions exponential in $n$, so the algorithm is not a general-purpose fast multiplicity calculator.

## Related notes

- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]
- [[Generalized Phase Estimation for Representation Projectors]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
- [[Dimension-Ratio Triage for Multiplicity Algorithms]]
