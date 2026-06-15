# DLP-Oracle Order Recovery from Random Exponent Differences

> **Source:** Banin--Tsaban, arXiv:1310.7903
> **Tags:** #trick #discrete-log #order-finding #semigroups

## What it does

Uses a DLP oracle to recover the order of a cyclic subgroup by comparing a sampled exponent with the oracle's returned representative.

## The trick

Let $h$ generate a cyclic group of order $m$, but suppose $m$ is unknown. Pick random
$$
k\in\{1,\ldots,N\}
$$
and query the DLP oracle on $h^k$ to obtain some representative
$$
k' := \log_h(h^k).
$$
The oracle's answer satisfies
$$
k'\equiv k\pmod m,
$$
so
$$
m\mid (k-k').
$$
Taking gcds of several nonzero differences $k-k'$ recovers $m$ with high probability.

In Banin--Tsaban's semigroup reduction, one first samples random powers $g^k$ deep in the cycle. Such a sample generates a subgroup of the cycle group $G$. Repeating for $O(\log\log n)$ samples and combining the subgroup orders by lcm recovers the full cycle length $n=|G|$ with high probability.

## When to reach for it

Use it when you have a discrete-log oracle but not an order oracle. It is a classical wrapper that converts log information into group-order information.

## Complexity

A constant number of DLP-oracle calls per sampled generator for a subgroup order, and $O(\log\log n)$ sampled cycle elements in the Banin--Tsaban application. Arithmetic overhead is gcd/lcm on integers of size polynomial in the chosen exponent bound.

## Caveat

The sampled element may generate only a proper subgroup. That is why the semigroup reduction repeats the procedure and combines orders. Also, the exponent range must be large enough that sampled semigroup powers are close to uniform on the eventual cycle.

## Related notes

- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]
- [[Classical Reduction of Semigroup DLP to Cycle-Group DLP]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
