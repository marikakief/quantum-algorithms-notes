# Constructive Semigroup Membership by One-Missing-Generator Search

> **Source:** Childs and Ivanyos, arXiv:1310.6238
> **Tags:** #trick #semigroups #constructive-membership #grover-search

## What it does

Solves multi-generator constructive membership in a finite abelian semigroup by Grover-searching all but one exponent and using shifted semigroup log for the missing exponent.

## The trick

Suppose $S=\langle g_1,\ldots,g_k\rangle$ is a finite abelian semigroup and
$$
x=g_1^{a_1}\cdots g_k^{a_k}
$$
for the lexicographically first exponent tuple $(a_1,\ldots,a_k)\in\mathbb N_0^k$. Childs and Ivanyos prove the volume bound
$$
(a_1+1)\cdots(a_k+1)\leq |S|.
$$
The proof is a pigeonhole argument: if the box $0\leq c_i\leq a_i$ were larger than $|S|$, two exponent tuples in the box would give the same semigroup element; subtracting the later tuple and adding the earlier one would produce a lexicographically smaller representation of $x$.

From the product bound, some index $j$ satisfies
$$
\prod_{i\neq j}(a_i+1)\leq |S|^{(k-1)/k}.
$$
For each possible $j$, Grover-search over the other $k-1$ exponents in the region
$$
\prod_{i\neq j}(a_i+1)\leq |S|^{(k-1)/k}.
$$
For each candidate tuple, set
$$
y=\prod_{i\neq j}g_i^{a_i}
$$
and solve
$$
x=yg_j^{a_j}
$$
using [[Shifted Semigroup Log as Dihedral Hidden Shift]].

## When to reach for it

- A representation problem has several commuting generators, but fixing all except one leaves a one-dimensional shifted-log problem.
- A lexicographically first solution gives a product-volume bound on the search region.
- The task is black-box enough that square-root search is the right outer layer.

## Complexity

For fixed $k$, the Grover search over a region of size $|S|^{(k-1)/k}$ costs
$$
|S|^{(k-1)/(2k)}=|S|^{1/2-1/(2k)}
$$
iterations. With Kuperberg-style shifted-log calls, the time is
$$
|S|^{1/2-1/(2k)+o(1)}.
$$
A query-efficient shifted-log subroutine gives
$$
|S|^{1/2-1/(2k)}\operatorname{poly}(\log |S|)
$$
queries.

## Caveat

The method is near-optimal for the black-box model in the source paper, but it is still exponential in $\log |S|$ for $k\geq 2$. The hidden-shift subroutine also carries the dihedral-HSP time bottleneck.

## Related notes

- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[Shifted Semigroup Log as Dihedral Hidden Shift]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]]
