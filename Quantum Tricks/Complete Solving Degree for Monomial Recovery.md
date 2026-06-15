# Complete Solving Degree for Monomial Recovery

> **Source:** Chen-Gao, arXiv:1712.06239
> **Tags:** #trick #macaulay-matrix #groebner-bases #polynomial-systems

## What it does

Strengthens solving degree so a Macaulay system recovers all bounded-degree monomials modulo the ideal, not merely enough information to compute a Gröbner basis.

## The trick

The usual solving degree of a polynomial system $F$ is the smallest $D$ such that Gaussian elimination on the degree-$D$ Macaulay system can produce the reduced Gröbner basis of $(F)$. Chen-Gao need more: their HHL step solves for monomial coordinates, so each relevant monomial must reduce correctly in the Macaulay linear system.

They define $D=\operatorname{CSdeg}(F)$ to mean that for every Gröbner-basis element $g$ and every monomial $m$ with $\deg(mg)\le D$, there are polynomials $h_i$ such that
$$
mg=\sum_i h_i f_i,
\qquad \deg(h_i f_i)\le D.
$$
This ensures that all degree-$\le D$ monomial consequences visible in the ideal are already visible inside the degree-$D$ Macaulay system.

For systems of the form
$$
F=\{g_1,\ldots,g_r\}\cup\{f_1,\ldots,f_n\},
$$
with leading monomials $\operatorname{lm}(f_i)=x_i^{d_i}$ and $\deg_{x_i}(g_j)<d_i$, they prove
$$
\operatorname{CSdeg}(F)\le d-2n+2\sum_{i=1}^n d_i,
\qquad d=\max_j\deg(g_j).
$$
For Boolean systems with $x_i^2-x_i$, this yields the clean bound
$$
\operatorname{CSdeg}(F_B\cup H_X)\le 3n.
$$

## When to reach for it

Use this when a Macaulay matrix is not just a way to derive a Gröbner basis, but is itself the linear system whose solution coordinates must correspond to monomial evaluations.

## Complexity

The degree bound controls the Macaulay dimensions. With the Boolean bound $D\le 3n$, the logarithmic matrix-size factors in the HHL step become $O(n\log n+\log r)$, while the dominant explicit dependence sits in sparseness and condition number.

## Caveat

A smaller classical solving degree can be misleading here. It may produce the right Gröbner basis while still failing to reduce monomial multiples needed for solution-state interpretation. Also, the bound is sufficient, not necessarily tight.

## Related notes

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Macaulay-HHL Pseudo-Solution States]]
- [[Sparse Parity-to-Complex Polynomial Embedding]]
