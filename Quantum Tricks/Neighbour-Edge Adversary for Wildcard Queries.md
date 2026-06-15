# Neighbour-Edge Adversary for Wildcard Queries

> **Source:** Ambainis and Montanaro, arXiv:1210.1148; Zhang strong weighted adversary
> **Tags:** #trick #adversary-method #lower-bound #query-complexity #wildcard-search

## What it does

Proves an $\Omega(\sqrt n)$ lower bound for wildcard search by weighting Hamming-neighbour input pairs and counting how many neighbours one wildcard query can separate.

## The trick

Let inputs be all $x\in\{0,1\}^n$. Put weight

$$
w(x,y)=1
$$

when $d(x,y)=1$, and zero otherwise. Then

$$
\operatorname{wt}(x)=n.
$$

A wildcard query is $(S,t)$ and returns 1 iff $x_S=t$. For a fixed input $x$, count neighbours $y$ at Hamming distance 1 whose query answer differs.

There are two cases:

- If the query accepts $x$, changing any coordinate in $S$ flips the answer, so the count is $|S|$.
- If the query rejects $x$, the answer can flip only if $x_S$ differs from $t$ in exactly one coordinate, so the count is at most 1.

The strong weighted adversary denominator is therefore at most the geometric mean of $|S|$ and 1, while the numerator has weight $n$. The bound gives

$$
\Omega(\sqrt n).
$$

## When to reach for it

- Query models where a yes-input query can be sensitive to many one-bit changes but a paired no-input is sensitive to few.
- Lower bounds for equality/subset-verification oracles.
- Turning a local sensitivity asymmetry into a square-root adversary bound.

## Complexity

The bound is

$$
\Omega\!\left(\sqrt{\frac{\operatorname{wt}(x)\operatorname{wt}(y)}{v(x,q)v(y,q)}}\right)=\Omega(\sqrt n).
$$

## Caveat

This lower bound is not tight to the $O(\sqrt n\log n)$ algorithm; it leaves a logarithmic gap. It also targets query complexity, not the cost of implementing queries or measurements.

## Related notes

- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]
- [[Wildcard-to-Group-Testing Block Reduction]]
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
