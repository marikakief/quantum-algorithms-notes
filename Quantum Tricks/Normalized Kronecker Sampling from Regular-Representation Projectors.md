# Normalized Kronecker Sampling from Regular-Representation Projectors

> **Source:** Bravyi--Chowdhury--Gosset--Havlicek--Zhu, arXiv:2302.11454; Moore--Russell--Sniady (2007)
> **Tags:** #trick #sampling #representation-theory #symmetric-group #Kronecker-coefficients

## What it does

Samples a partition $\lambda$ with probability proportional to the normalized Kronecker coefficient $d_\lambda g_{\mu\nu\lambda}/(d_\mu d_\nu)$.

## The trick

Prepare the maximally mixed state on the image of the left-regular projectors $\Pi^L_\mu$ and $\Pi^L_\nu$:
$$
\frac{\Pi^L_\mu}{d_\mu^2}\otimes \frac{\Pi^L_\nu}{d_\nu^2}.
$$
This can be done by preparing a mixed state over Fourier-basis labels $|\mu,i,j\rangle$ and applying the inverse symmetric-group QFT.

Now measure the irrep label $\lambda$ for the product representation on $\mathbb C S_n\otimes\mathbb C S_n$. The outcome distribution is
$$
p(\lambda)=\frac{d_\lambda g_{\mu\nu\lambda}}{d_\mu d_\nu}.
$$
The proof is a trace calculation using the projector formula
$$
\Pi_\lambda = \frac{d_\lambda}{n!}\sum_{g\in S_n}\chi_\lambda(g)R(g)\otimes R(g)
$$
and the character formula for $g_{\mu\nu\lambda}$.

## When to reach for it

Use this when relative-error counting is too hard but additive sampling of a normalized multiplicity distribution is enough. It is also useful as a subroutine for finding likely tensor-product components.

## Complexity

For $S_n$, sample generation costs $\operatorname{poly}(n)$ using the symmetric-group QFT and generalized phase estimation. Repeating $O(1/\epsilon^2)$ times estimates $p(\lambda)$ to additive error $\epsilon$, which gives an additive estimate of
$$
g_{\mu\nu\lambda}
$$
with error
$$
\delta=\epsilon\frac{d_\mu d_\nu}{d_\lambda}.
$$

## Caveat

The induced additive error for the unnormalized Kronecker coefficient can be enormous when $d_\mu d_\nu/d_\lambda$ is large. This becomes an exact algorithm only in special regimes, for example when $\min(d_\mu,d_\nu,d_\lambda)$ is polynomially bounded and $\epsilon$ is chosen so that $\delta<1/2$.

## Related notes

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
- [[Generalized Phase Estimation for Representation Projectors]]
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
