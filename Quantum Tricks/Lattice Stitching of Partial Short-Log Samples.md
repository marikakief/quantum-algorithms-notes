# Lattice Stitching of Partial Short-Log Samples

> **Source:** Martin Ekerå and Johan Håstad, arXiv:1702.00249
> **Tags:** #trick #lattices #discrete-log #post-processing

## What it does

Recovers a short hidden integer from several noisy modular linear relations by a fixed-dimensional closest-vector search.

## The trick

Assume we have $s$ good samples $(j_i,k_i)$ satisfying

$$
\left|\{dj_i+2^m k_i\}_{2^{\ell+m}}\right|\le 2^{m-2}
$$

for the same unknown $0<d<2^m$. Build the lattice $L\subset\mathbb Z^{s+1}$ with rows

$$
(j_1,\ldots,j_s,1),\quad
(2^{\ell+m},0,\ldots,0,0),\quad\ldots,\quad
(0,\ldots,0,2^{\ell+m},0).
$$

Set

$$
\vec v=\bigl(\{-2^m k_1\}_{2^{\ell+m}},\ldots,\{-2^m k_s\}_{2^{\ell+m}},0\bigr).
$$

Then a lattice vector whose last coordinate is $d$ lies within

$$
\sqrt{s/4+1}\,2^m
$$

of $\vec v$. Enumerate all lattice vectors in that ball and test the last coordinate by checking $x=[d]g$.

## When to reach for it

Use it when quantum sampling gives several approximate congruences involving the same small secret, and each individual sample is too weak to determine the secret alone.

## Complexity

For fixed $s$, the lattice dimension is $s+1$, so enumeration is polynomial in the input bit length with constants depending on $s$. The paper bounds the chance that the lattice has a very short spurious vector by $2^{-s-1}$ over the good-sample choices.

## Caveat

This is not a free lunch if $s$ is allowed to grow. The paper's polynomial-time claim treats $s$ as fixed; otherwise subset selection and lattice enumeration need new accounting.

## Related notes

- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]
- [[Short Discrete Logarithm by Truncated Dual-Register QFT]]
- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
