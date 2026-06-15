# Convex Grid Normalisation for Algebraic-Integer Candidate Search

> **Source:** Ross and Selinger, arXiv:1403.2975
> **Tags:** #trick #gate-synthesis #Clifford-T #algebraic-number-theory #lattice-enumeration

## What it does

Finds algebraic-integer candidates satisfying simultaneous region constraints on an element and its Galois conjugate, without scanning a large lattice box.

## The trick

Suppose the candidate must satisfy

$$
u\in A,
\qquad
u^\bullet\in B,
\qquad
u\in \mathbb D[\omega],
$$

where $A,B\subset\mathbb R^2$ are bounded convex sets and $(-)^\bullet$ sends $\sqrt 2\mapsto -\sqrt 2$. For a fixed denominator exponent $k$, scale to

$$
u\in \frac{1}{\sqrt 2^{\,k}}\mathbb Z[\omega].
$$

The naive enumeration is bad when $A$ or $B$ is long and thin. Instead:

1. Enclose $A$ and $B$ in ellipses with constant-factor area overhead.
2. Apply a special grid operator $G$ so that
   $$
   \nu\mapsto G\nu,
   \qquad
   \nu^\bullet\mapsto G^\bullet\nu^\bullet
   $$
   preserves solutions.
3. Iterate a skew-reduction step until both transformed regions are close to upright boxes.
4. Enumerate the transformed grid by reducing to one-dimensional constraints over $\mathbb Z[\sqrt 2]$.
5. Map solutions back by $G^{-1}$.

The key is that the transformation respects both embeddings at once: it changes the geometry but not the algebraic relation between $\nu$ and $\nu^\bullet$.

## When to reach for it

- Candidate selection in exact or approximate synthesis over cyclotomic rings.
- Any search over algebraic integers where two real/complex embeddings must lie in prescribed convex regions.
- Thin-cap approximation problems where ordinary lattice enumeration wastes time on the bounding box.

## Complexity

For $M$-upright convex regions, Ross--Selinger's enumeration costs

$$
O(\log(1/M))
$$

overall arithmetic operations, plus a constant number per solution produced. For the $R_z$ synthesis cap $R_\varepsilon$, the relevant uprightness is $M=\Omega(\varepsilon^4)$, so the geometric normalisation cost is $O(\log(1/\varepsilon))$.

## Caveat

You need effective convex descriptions and a special grid-operator family compatible with the ring embeddings. The trick is not a generic lattice shortest-vector method; it is tailored to rings like $\mathbb Z[\omega]$ with the $\sqrt 2$ conjugation structure.

## Related notes

- [[Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014) — Paper Notes]]
- [[Norm-Equation Completion of Clifford+T Matrix Entries]]
- [[Two-Phase Reduction for Global-Phase Clifford+T Synthesis]]
