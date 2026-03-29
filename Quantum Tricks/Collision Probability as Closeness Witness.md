# Collision Probability as Closeness Witness

> **Source:** Bravyi, Harrow, Hassidim, arXiv:0907.3920
> **Tags:** #trick #distribution-testing #collision-problem #orthogonality

## What it does
Tests whether two distributions are orthogonal (disjoint supports) or close in $L_1$ distance by checking whether random samples from one distribution hit the support of the other.

## The trick
Given distributions $p, q$ on $[N]$, draw $M$ samples from $p$ to form a set $A \subseteq [N]$. Compute the "collision probability" $q_A = \sum_{i \in A} q_i$.

**One-sided guarantee:**
- If $p, q$ are orthogonal (supports disjoint): $q_A = 0$ with certainty.
- If $\|p - q\|_1 \leq 2 - \varepsilon$: the supports overlap, and $q_A \geq \varepsilon^3 M/(2^{11} N)$ with probability $\geq 1/2$.

The second bound (Lemma 6) works by showing that elements $i$ where both $q_i \geq (\varepsilon/4) p_i$ and $p_i > \varepsilon/(32N)$ constitute a fraction $\geq \varepsilon/16$ of the samples, contributing enough to $q_A$.

Estimate $q_A$ using [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]] at cost $O(1/\sqrt{q_A}) = O(\sqrt{N/M})$ queries to $q$. The total query count $M + \sqrt{N/M}$ is minimised at $M \sim N^{1/3}$, giving $O(N^{1/3})$ total.

## When to reach for it
When testing for support disjointness or overlap between two distributions accessed via oracles. The one-sided nature (exact zero for orthogonal distributions, guaranteed positive for overlapping) makes this a clean decision procedure. Also applies to collision-type problems more broadly.

## Complexity
$O(N^{1/3})$ queries total. This is tight: the $\Omega(N^{1/3})$ lower bound follows from reduction to the collision problem (Aaronson-Shi).

## Caveat
The detection probability depends on $\varepsilon^3$, so testing distributions that are close to orthogonal (small $\varepsilon$) gets expensive. Also, the algorithm requires QRAM to implement the membership oracle for the set $A$, which may be costly in practice.

## Related notes
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]]
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]
- [[Amplitude Estimation for Distribution Property Testing]]
- [[Sample-then-Estimate Balancing for Query Optimisation]]
