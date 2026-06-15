# Plethysm Coefficient Extraction by Ordered-Composition Polytopes

> **Source:** Panova, arXiv:2502.20253
> **Tags:** #trick #representation-theory #plethysm #lattice-point-counting

## What it does

Turns the basic plethysm coefficient $a^\lambda_{d,m}$ in $h_d[h_m]$ into a signed sum of integer-point counts in bounded-dimensional polytopes.

## The trick

Work in $k=\ell(\lambda)$ variables. Expand $h_m(x_1,\ldots,x_k)$ as a lexicographically ordered list of monomials indexed by weak compositions $b=(b_1,\ldots,b_k)$ of $m$. A term of $h_d[h_m]$ chooses distinct compositions $b^1<\cdots<b^r$ with positive multiplicities $c_1+\cdots+c_r=d$.

The lexicographic constraints are stratified by a vector $\bar j\in[k]^{r-1}$ recording the first coordinate where $b^i$ and $b^{i+1}$ differ. After multiplying by the Vandermonde determinant and using Weyl's formula for Schur functions, coefficient extraction gives
$$
a^\lambda_{d,m}=\sum_{\sigma\in S_k}\operatorname{sgn}(\sigma)
\sum_{r=1}^d\sum_{c_1+\cdots+c_r=d}\sum_{\bar j\in[k]^{r-1}}
|Q(\bar j,c,\lambda+\delta(k)-\sigma(\delta(k)))|.
$$
Here $Q(\bar j,c,\alpha)$ is the polytope of integer matrices $(b^i_j)$ satisfying the composition equations, the lexicographic stratum constraints, and
$$
c_1b^1+\cdots+c_rb^r=\alpha .
$$
When $d$ and $k$ are fixed, Barvinok counting applies directly.

## When to reach for it

Use this when a symmetric-function coefficient has a monomial expansion with an ordering constraint and fixed effective dimension. It is best suited to plethysm coefficients where either $d$ and $\ell(\lambda)$ are fixed or $\lambda$ has bounded aft.

## Complexity

For $k=\ell(\lambda)$,
$$
O\!\left(k!(2k)^{d-1}(dm)^{dk}\log(dk)\right)
$$
via fixed-dimensional integer-point counting. In the bounded-aft case, Panova trims the large trivial composition and obtains a polynomial-time algorithm when $f_\lambda\le n^c$.

## Caveat

This is again a signed formula, not a positive rule for plethysm. It also targets the basic case $a^\lambda_{d,m}$ from $s_{(d)}[s_{(m)}]$; general plethysm coefficients need extra work.

## Related notes

- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]
- [[Aft-Bounded Partition Regime for Polynomial Specht Dimension]]
