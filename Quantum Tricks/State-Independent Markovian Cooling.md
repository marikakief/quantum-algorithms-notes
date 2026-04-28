# State-Independent Markovian Cooling

> **Source:** Raeisi, Kieferová, Mosca, arXiv:1902.04439
> **Tags:** #trick #algorithmic-cooling #markov-chains #fixed-point

## What it does
Replaces adaptive entropy-compression steps by a single fixed operation whose stationary distribution is already the desired cooled state.

## The trick
Instead of re-optimizing the compression unitary from the current state at each round, pick a fixed unitary $U$ such that the induced map on diagonal populations is a Markov chain with the target distribution $\pi$ as its unique fixed point.

Then alternate:
1. reset the refrigerator / ancilla subsystem to a known bath state,
2. apply the same $U$ again.

If the chain is irreducible enough and contracts toward $\pi$, repeated application drives the system to the desired asymptotic state without any state estimation.

The specific instance in [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]] uses adjacent swaps after each reset step, but the more general pattern is broader: engineer the fixed point, then let mixing do the work.

## When to reach for it
- When the “optimal” adaptive update depends on tomography or expensive classical feedback.
- When you only care about the asymptotic state, not the exact best next step.
- When a time-homogeneous Markov description is easier to analyse than adaptive control.

## Complexity
Per iteration depends on the chosen fixed unitary. In the TSAC example it is $O(n^2)$ gates, while the number of iterations to mix can still be exponential.

## Caveat
You often trade per-step simplicity for slower convergence. Matching the optimal fixed point does not mean matching the best finite-time schedule.

## Related notes
- [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]]
- [[Lexicographic Two-Sort Compression]]
