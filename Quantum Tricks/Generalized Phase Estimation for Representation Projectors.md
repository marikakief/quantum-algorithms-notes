# Generalized Phase Estimation for Representation Projectors

> **Source:** Bravyi--Chowdhury--Gosset--Havlicek--Zhu, arXiv:2302.11454; Harrow, arXiv:quant-ph/0512255
> **Tags:** #trick #phase-estimation #representation-theory #quantum-fourier-transform

## What it does

Measures the irrep label inside any efficiently controlled unitary representation of a finite group.

## The trick

Let $\rho:G\to U(\mathcal H)$ be a unitary representation, and let $\lambda$ label an irrep of $G$ with character $\chi_\lambda$ and dimension $d_\lambda$. The representation projector is
$$
\Pi_\lambda=\frac{d_\lambda}{|G|}\sum_{g\in G}\chi_\lambda(g)\rho(g).
$$
To measure $\{\Pi_\lambda\}$:

1. Prepare an ancilla group register in the trivial Fourier state, then inverse-QFT it to
   $$
   \frac{1}{\sqrt{|G|}}\sum_{g\in G}|g\rangle .
   $$
2. Apply the controlled inverse action
   $$
   |g\rangle|\psi\rangle\mapsto |g\rangle\rho(g)^{-1}|\psi\rangle .
   $$
3. Perform weak Fourier sampling on the group register.
4. Apply the controlled forward action to decouple the group register.

Conditioned on observing irrep label $\lambda$, the data register has been projected by $\Pi_\lambda$.

For $G=S_n$, Bravyi--Chowdhury--Gosset--Havlicek--Zhu use this with the threefold tensor-product representation $\rho(\sigma)=\sigma\otimes\sigma\otimes\sigma$ to test the trivial irrep, and with the conjugation representation to study character row sums.

[[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes|Larocca--Havlicek (2025)]] use the same measurement pattern in a subgroup-restriction setting: the target is a $G$-isotypic state, the controlled action is restricted to $H\subseteq G$, and the $H$-Fourier label probability is a branching multiplicity times a dimension ratio.

## When to reach for it

Reach for this whenever the quantity of interest is “the part of a state transforming as irrep $\lambda$” under a group action, but directly expanding characters is impossible. It is the nonabelian analogue of using phase estimation to measure eigenspaces of an abelian action.

## Complexity

The cost is the cost of the group QFT/weak Fourier sampling plus two controlled applications of $\rho(g)$, all at the desired precision. For $S_n$ with explicit permutation registers, this is $\operatorname{poly}(n)$.

## Caveat

The method needs an efficient QFT or at least efficient weak Fourier sampling for the group, and an efficient controlled-$\rho$ gate. For arbitrary nonabelian groups, those are exactly where the hard work can hide.

## Related notes

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]
- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
- [[Character Row Sums from Conjugation-Representation Projectors]]
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
