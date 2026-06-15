# Random Bilinear Compression for Submatrix Product Tests

> **Source:** Buhrman-Špalek, arXiv:quant-ph/0409035
> **Tags:** #trick #random-projection #matrix-verification #quantum-walks

## What it does

Compresses a submatrix product identity to one random scalar equation while preserving a constant chance of detecting any nonzero error over an integral domain.

## The trick

Suppose a quantum walk vertex stores row and column subsets $R,S\subseteq[n]$. The direct markedness test for matrix product verification asks whether

$$
A|_R B|_S=C^S_R.
$$

Computing the full $k\times k$ product is too expensive. Instead choose random vectors $p,q$ and store

$$
a_R=p|_R A|_R,\qquad b_S=B|_S q|_S,\qquad c_{R,S}=p|_R C^S_R q|_S.
$$

Then test the scalar condition

$$
a_R\cdot b_S\stackrel{?}{=}c_{R,S}.
$$

Equivalently, this checks whether

$$
p|_R(A|_RB|_S-C^S_R)q|_S=0.
$$

If $(R,S)$ contains a wrong entry and the entries lie in an integral domain, random $p,q$ make this scalar nonzero with probability at least $1/4$ for a fixed marked pair. Buhrman-Špalek then show that with probability greater than $1/8$ over $p,q$, at least an $1/8$ fraction of the marked pairs remain revealing. Repeating the walk test a constant number of times restores bounded error.

## When to reach for it

Use it when a quantum walk needs a phase oracle for a matrix/submatrix equality, but the full equality check is too expensive. It is the two-sided analogue of Freivalds-style random projection: compress both row and column dimensions before doing the quantum phase test.

## Complexity

Initialisation costs $2kn+k^2$ arithmetic operations to compute $a_R,b_S,c_{R,S}$. After a Johnson-graph update that changes one row and one column, the stored compressed data can be updated using $O(n)$ queries/time, and the phase test costs $O(n)$.

## Caveat

The trick needs arithmetic over an integral domain. It does not directly apply to Boolean semiring multiplication, where cancellation/projection behaviour is different. It also converts marked vertices into revealing vertices only with constant probability, so the surrounding algorithm must tolerate repeated trials.

## Related notes

- [[Quantum Verification of Matrix Products (Buhrman-Špalek 2005) — Paper Notes]]
- [[Johnson-Product Quantum Walk for Matrix Product Verification]]
- [[Sparse-Output Product via Verification and Correction]]
- [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes]]
