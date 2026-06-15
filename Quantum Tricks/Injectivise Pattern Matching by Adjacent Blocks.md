# Injectivise Pattern Matching by Adjacent Blocks

> **Source:** Montanaro, arXiv:1408.1816
> **Tags:** #trick #pattern-matching #hidden-shift #average-case #query-complexity

## What it does

Turns local substrings into symbols so that a non-injective pattern-matching instance becomes an injective hidden-shift instance.

## The trick

Given a string $S:[n]^d\to\Sigma$, define

$$
S^{\triangleright k}(s)=S_{s,k}\in\Sigma^{k^d},
$$

where $S_{s,k}$ is the $k\times\cdots\times k$ block of $S$ beginning at $s$.

For random $S$, short blocks are very likely to be unique. A union bound gives

$$
\Pr[\upsilon(S)\ge (3d\log_q n)^{1/d}]\le 1/n^d.
$$

So after grouping logarithmic-volume blocks, matching $P$ in $T$ can be treated as aligning two injective functions. That unlocks hidden-shift algorithms.

## When to reach for it

- Pattern matching where the raw alphabet is too small for injectivity.
- Average-case string/image matching over random data.
- Reductions from local matching to [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|hidden shift]].

## Complexity

A query to $S^{\triangleright k}$ costs $k^d$ queries to $S$. For random strings, $k^d=O(d\log_q n)$ suffices with high probability.

## Caveat

This is useful only if the block-query overhead is smaller than the gain from hidden-shift refinement. It also assumes the data is random enough that repeated blocks are rare.

## Related notes

- [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]]
- [[Coarse Offset Search plus Hidden Shift Refinement]]
- [[Quantum Sieve for Labelled Qubits]]
