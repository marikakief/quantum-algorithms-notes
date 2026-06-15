# Shifted Semigroup Log as Dihedral Hidden Shift

> **Source:** Childs and Ivanyos, arXiv:1310.6238
> **Tags:** #trick #semigroups #hidden-shift #dihedral-hsp

## What it does

Reduces the shifted semigroup equation $x=yg^a$ to a dihedral hidden subgroup problem once the orbit of $y$ has entered its cycle.

## The trick

For fixed $y,g\in S$, study the sequence
$$
yg,yg^2,yg^3,\ldots
$$
It has an index $\tilde t$ and period $\tilde r$, found by the same method as [[Semigroup Index-Period Detection by Tail-Skipping Period Finding]]. If $x$ is in the tail, the exponent can be found by binary search.

If $x$ is in the cycle, write
$$
x=yg^{\tilde t+\ell}
$$
for some $\ell\in\mathbb Z_{\tilde r}$. Define a function on the dihedral group $\mathbb Z_2\ltimes\mathbb Z_{\tilde r}$:
$$
f(0,j)=yg^{\tilde t+j},\qquad f(1,j)=xg^j.
$$
Then
$$
f(1,j)=xg^j=yg^{\tilde t+\ell+j}=f(0,j+\ell),
$$
so $f$ hides the reflection subgroup
$$
\langle(1,\ell)\rangle.
$$
A dihedral-HSP solver recovers $\ell$, hence $a=\tilde t+\ell$.

## When to reach for it

- You have two orbit maps, $j\mapsto yg^j$ and $j\mapsto xg^j$, and one is a shift of the other.
- The ambient algebra lacks inverses, so $y^{-1}x$ cannot be formed.
- The problem is closer to hidden shift than to ordinary discrete log.

## Complexity

Using [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg's sieve]], the time is
$$
2^{O(\sqrt{\log \tilde r})}\leq 2^{O(\sqrt{\log |S|})}.
$$
Using query-efficient dihedral-HSP methods, the query complexity can be $\operatorname{poly}(\log |S|)$, but known post-processing is not polynomial time.

## Caveat

This is not a polynomial-time algorithm unless the dihedral hidden subgroup problem is made polynomial-time. The reduction also goes in the algorithmic direction: shifted semigroup log is solved by DHSP machinery; it does not by itself prove DHSP hardness.

## Related notes

- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[Cyclic Group Extraction from a Semigroup Cycle]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes]]
- [[Quantum Sieve for Labelled Qubits]]
- [[Coset Sampling via Fourier Transform]]
