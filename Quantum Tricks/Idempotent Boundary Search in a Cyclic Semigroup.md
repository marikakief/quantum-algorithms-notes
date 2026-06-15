# Idempotent Boundary Search in a Cyclic Semigroup

> **Source:** Banin--Tsaban, arXiv:1310.7903
> **Tags:** #trick #semigroups #discrete-log #binary-search

## What it does

Finds where a periodic semigroup power sequence has entered its cycle by turning idempotence into a monotone search predicate.

## The trick

Suppose $g$ has cycle length $n$, and let $t$ be the least integer with $tn>\ell$, where $g^{\ell+n}=g^\ell$ starts the eventual cycle. The cycle group has identity
$$
e=g^{tn}.
$$
For $s\geq t$,
$$
g^{sn}=g^{tn}=e,
$$
while before the boundary this equality has not stabilised. Since the identity of a group is the unique idempotent, test
$$
P(s):=[g^{sn}=(g^{sn})^2].
$$
Then $P(s)$ is false before the boundary and true from $t$ onward.

Algorithmically:

1. Start with $b=1$.
2. Double $b$ until $g^{bn}$ is idempotent.
3. Binary-search between $b/2$ and $b$ for the first idempotent power.

The same monotone-boundary idea is used again when a target $h=g^x$ lies in the tail: search for the least $b$ with $g^{bn}h\in G$.

## When to reach for it

Use it whenever an algebraic process has a tail followed by a cycle and you can test membership in the stable cycle. It is especially useful when there is no inverse operation, but multiplying by a known period step moves tail elements monotonically toward the cycle.

## Complexity

$O(\log t)$ predicate tests after the doubling phase. With precomputed powers $g^{2^i}$, each test uses polynomially many semigroup multiplications, and often only a small number of assembled products.

## Caveat

The predicate must really be monotone. Here monotonicity follows from periodicity and the specific step size $n$; using the wrong period can produce false structure.

## Related notes

- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]
- [[Classical Reduction of Semigroup DLP to Cycle-Group DLP]]
- [[Semigroup Index-Period Detection by Tail-Skipping Period Finding]]
- [[Cyclic Group Extraction from a Semigroup Cycle]]
