# Small-Kernel Extension Closure for HSP

> **Source:** Moore--Rockmore--Russell--Schulman, arXiv:quant-ph/0211124
> **Tags:** #trick #hidden-subgroup-problem #group-extensions #quotients

## What it does

Solves the HSP on a group extension when the kernel is small and the quotient HSP is already efficient.

## The trick

Suppose

$$
K\triangleleft G,
\qquad G/K\cong H,
$$

where $|K|=\operatorname{poly}(\log|H|)$ and the HSP on $H$ is efficient.

For a hidden subgroup $L\leq G$:

1. Find $L\cap K$ by querying the oracle on all elements of the small kernel $K$.
2. Choose a transversal $t(h)$ for cosets of $K$.
3. Define a quotient oracle

$$
F'(h)=\{F(g):g\in t(h)K\}.
$$

This hides the projected subgroup $LK/K\leq H$.

4. Solve the HSP on $H$ to get generators $T$ for $LK/K$.
5. For each $h\in T$, search the small coset $t(h)K$ for a representative $\eta(h)\in L$.
6. Combine generators for $L\cap K$ with the representatives $\eta(T)$.

## When to reach for it

Use it when a nonabelian group is an extension by a genuinely small normal subgroup: small commutator subgroup, Hamiltonian-group cases, or other nearly-quotient problems.

## Complexity

Each quotient-oracle call costs $|K|$ original queries. The method is polynomial only when $|K|$ is polynomial in the input length.

## Caveat

The construction cannot be iterated through large kernels. When $K$ is large, finding representatives in $t(h)K$ can become the hard part, as in dihedral-style hidden conjugate problems.

## Related notes

- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
