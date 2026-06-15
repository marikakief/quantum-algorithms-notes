# Induced-Representation Multiplicity Sampling by Coset Embedding

> **Source:** Larocca--Havlicek, arXiv:2407.17649
> **Tags:** #trick #representation-theory #fourier-sampling #hidden-subgroup-problem

## What it does

Estimates an induced-representation multiplicity by embedding a coset register and subgroup regular register into the full group algebra before weak Fourier sampling.

## The trick

For $H\subseteq G$, Frobenius reciprocity says
$$
\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G)
=
\operatorname{mult}(r_G^\alpha,r_H^\beta\uparrow_H^G).
$$
Instead of restricting a $G$-irrep, prepare the $H$-isotypic state
$$
\rho_H^\beta={\Pi_H^\beta\over d_\beta^2}
$$
in a subgroup register $\mathbb C[H]$, and tensor it with a maximally mixed coset register $\mathbb C[G/H]$. Choose coset representatives $t$ and apply
$$
U_E: |t\rangle|h\rangle\mapsto |th\rangle.
$$
This identifies $\mathbb C[G/H]\otimes\mathbb C[H]$ with $\mathbb C[G]$. Applying the $G$-QFT and measuring the $G$-irrep label yields
$$
\Pr[\alpha]
={|H|d_\alpha\over |G|d_\beta}
\operatorname{mult}(r_G^\alpha,r_H^\beta\uparrow_H^G).
$$

## When to reach for it

Use it when the induced side has the better dimension ratio, or when the natural state preparation lives over $H$ rather than $G$. It also clarifies when hidden-subgroup Fourier sampling is secretly an induced-representation multiplicity sampler: the standard HSP coset state is the case $\beta=\mathrm{triv}$.

## Complexity

With a $G$-QFT of cost $G$ and a state-preparation circuit of cost $R$ for $\rho_H^\beta$, the sampling/rounding cost is
$$
O\!\left((G+R)\left({d_\beta |G|\over d_\alpha |H|}\right)^2\right).
$$
Compare this with the restriction-side cost $O((d_\alpha/d_\beta)^2)$, ignoring shared implementation factors. The restriction version wins when
$$
\left({d_\alpha\over d_\beta}\right)^2 < {|G|\over |H|}.
$$

## Caveat

The embedding $|t\rangle|h\rangle\mapsto |th\rangle$ needs efficient coset representatives and group multiplication. For non-black-box groups this may be easy; for opaque encodings it can dominate the algorithm.

## Related notes

- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]
- [[Multiplicity Estimation by Restricted-Isotypic Fourier Sampling]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
