# Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector

> **Source:** Bravyi--Chowdhury--Gosset--Havlicek--Zhu, arXiv:2302.11454
> **Tags:** #trick #representation-theory #symmetric-group #counting-complexity #QMA

## What it does

Turns a tensor-product multiplicity into the rank of an efficiently checkable projector.

## The trick

For $\mu,\nu,\lambda\vdash n$, work on three copies of the regular representation space $\mathbb C S_n$. First project the three registers onto left-regular Fourier sectors $\mu,\nu,\lambda$:
$$
\Pi^L_\mu\otimes \Pi^L_\nu\otimes \Pi^L_\lambda .
$$
Then project onto invariance under the diagonal left action of $S_n$:
$$
Q=\frac{1}{n!}\sum_{\sigma\in S_n}\sigma\otimes\sigma\otimes\sigma .
$$
Since these projectors commute, their product
$$
P_{\mu\nu\lambda}=Q(\Pi^L_\mu\otimes \Pi^L_\nu\otimes \Pi^L_\lambda)
$$
is a projector. Character orthogonality gives
$$
\operatorname{Tr}(P_{\mu\nu\lambda})=d_\mu d_\nu d_\lambda g_{\mu\nu\lambda}.
$$
So $g_{\mu\nu\lambda}$ is not just a formal multiplicity: it is a scaled dimension of a concrete quantum-verifiable subspace.

## When to reach for it

Use this when a multiplicity is defined by a tensor-product decomposition and the group has:

- an efficient Fourier-label measurement;
- efficient controlled action of the group on the relevant registers;
- an invariant-subspace condition expressible as a trivial-irrep test.

This is a good pattern for turning algebraic multiplicities into $\#\mathrm{BQP}$-style counts.

## Complexity

For $S_n$, the projector is measured with $\operatorname{poly}(n)$ gates using Beals's efficient symmetric-group QFT plus efficient reversible permutation multiplication. Exact counting of the rank is a $\#\mathrm{BQP}$ task; relative-error approximation reduces to QXC.

## Caveat

The construction gives a verifier/counting interpretation, not a fast relative-error estimator. If the rank is exponentially small or QXC is hard, this does not by itself make $g_{\mu\nu\lambda}$ easy to compute.

## Related notes

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]
- [[Generalized Phase Estimation for Representation Projectors]]
- [[Normalized Kronecker Sampling from Regular-Representation Projectors]]
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
