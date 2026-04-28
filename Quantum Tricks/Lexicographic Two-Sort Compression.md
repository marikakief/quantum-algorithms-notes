# Lexicographic Two-Sort Compression

> **Source:** Raeisi, Kieferová, Mosca, arXiv:1902.04439
> **Tags:** #trick #algorithmic-cooling #permutations #compression

## What it does
Approximates full sorting by repeatedly swapping neighboring entries in a fixed pattern, while keeping the boundary states pinned.

## The trick
On a basis ordered lexicographically, define a permutation that swaps $(1,2)$, $(3,4)$, $(5,6)$, and so on, while leaving the first and last basis states unchanged. After a reset step that interleaves each population with bath weights $e^{\pm \epsilon}$, this adjacent-swap pattern pushes weight in the same coarse direction as the full partner-pairing sort.

Repeated application is enough to recover the same asymptotic HBAC fixed point, even though each individual step is much weaker than a full sort.

## When to reach for it
- When a full sort is too expensive or state-dependent.
- When the stationary distribution matters more than greedy one-step improvement.
- When you want a permutation with a very regular circuit structure.

## Complexity
The permutation itself is simple. In the TSAC circuit from [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]], it can be implemented with $O(n^2)$ gates using a cyclic shift plus boundary correction.

## Caveat
This is not a general replacement for sorting. Its usefulness depends on the specific reset-and-compress structure of the dynamics.

## Related notes
- [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]]
- [[State-Independent Markovian Cooling]]
- [[SHIFT Operator via QFT Phases]]
