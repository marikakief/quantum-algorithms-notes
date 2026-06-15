# Local-Filter Johnson Walk for Bucket Pair Finding

> **Source:** Chailloux--Loyer, arXiv:2105.05608
> **Tags:** #trick #quantum-walks #johnson-graph #lattice-sieving #pair-finding

## What it does

Finds close pairs inside a large bucket by running a Johnson-graph quantum walk whose vertices store a second layer of local filters.

## The trick

Start with a bucket of $N^c$ candidate residual vectors $L_y$. Walk on the Johnson graph
$$
J(N^c,N^{c_1}),
$$
so a vertex contains a subset $L_y^v$ of size $N^{c_1}$.

The extra data in the vertex is the point. Sample an inner spherical code $C_2$ and store, for each $\vec t\in C_2$,
$$
J^v(\vec t)=f_\beta(\vec t)\cap L_y^v.
$$
A vertex is marked when some inner bucket contains a pair whose residual angle is below the threshold from [[Residual-Angle Reduction Inside Spherical Filters]].

This converts pair finding into MNRS quantum-walk search with cost
$$
S+\frac{1}{\sqrt\varepsilon}\left(\frac{1}{\sqrt\delta}U+C\right),
$$
where, in the full inner-code version,
$$
S=N^{c_1+\rho_0+o(1)},\qquad
U=N^{\max\{\rho_0,(\rho_0+c_2)/2\}+o(1)},\qquad
\delta\approx N^{-c_1}.
$$
The inner filters make the marked check/update local: after inserting a new point, only the inner buckets that point survives need to be inspected.

## When to reach for it

Use this when all three conditions hold:

1. solutions are pairs inside a bucket or list;
2. naive pair checking costs too much;
3. there is a geometry/hash/filter family that makes true pairs collide in a smaller local structure.

The pattern is not limited to lattices. It is a useful design for collision, near-collision, and structured pair-search tasks where a Johnson walk alone would have a bad checking cost.

## Complexity

The cost depends on the bucket exponent $c$, vertex size exponent $c_1$, inner-bucket exponent $c_2$, and collision-cover exponent $\rho_0$. In Chailloux--Loyer's first optimisation the full version gives
$$
N^{1.2555+o(1)}=2^{0.2605d+o(d)}
$$
for the whole SVP sieve.

## Caveat

The trick needs coherent data structures for insertion/deletion in superposition, i.e. QRAQM-style assumptions. It also needs worst-case control on inner bucket size, which Chailloux--Loyer handle by truncating $J^v(\vec t)$ to $\widetilde J^v(\vec t)$.

## Related notes

- [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]]
- [[Residual-Angle Reduction Inside Spherical Filters]]
- [[Marked-Vertex Thinning in Quantum Walk Search]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Johnson-Product Quantum Walk for Matrix Product Verification]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
