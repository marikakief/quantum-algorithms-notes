# Gentle Search via Quantum OR

> **Source:** Scott Aaronson, arXiv:1711.01053 (Shadow Tomography, STOC 2018)
> **Tags:** #trick #shadow-tomography #quantum-search #gentle-measurement #sample-complexity

## What it does
Locates a "violating" measurement among $M$ candidates — one where the true state disagrees with a hypothesis by $\ge \varepsilon$ — using only $O(\log^4 M / \varepsilon^2)$ copies of the state, without destroying enough of the state to prevent further use.

## The trick

**Goal:** Given copies of $\rho$ and $M$ two-outcome tests $E_1, \ldots, E_M$, find some $j$ such that $\operatorname{Tr}(E_j \rho) \ge c - \varepsilon$, promised that some $i$ has $\operatorname{Tr}(E_i \rho) \ge c$.

**Steps:**
1. Use [[Quantum OR Subroutine]] as the decision oracle: given a subset $S \subseteq [M]$, distinguish "some $E_i, i \in S$, accepts $\rho$ with prob $\ge c$" from "all accept with prob $\le c - \varepsilon/\log M$" using $O(\log|S| / (\varepsilon/\log M)^2) = O(\log M \cdot \log^2 M / \varepsilon^2)$ copies.
2. Run binary search over the index set $[M]$: test the left half, recurse into the half containing a good index. Depth $O(\log M)$.
3. Each level of the search degrades the gap by a $1/\log M$ factor (to maintain soundness under $O(\log M)$ sequential applications); account for this by adjusting the Quantum-OR threshold.
4. Total copies: $O(\log M)$ levels × $O(\log^2 M / \varepsilon^2)$ per level = $O(\log^3 M / \varepsilon^2)$, plus polylog correction factors from error amplification → $O(\log^4 M / \varepsilon^2)$.

**Gentleness:** The [[Gentle Measurement Lemma]] ensures each Quantum-OR call disturbs $\rho$ by $O(\sqrt{p})$ in trace distance where $p$ is the acceptance probability of the "failure" event. Summed over $O(\log M)$ calls, the cumulative damage is small, so the state remains usable for further iterations.

## When to reach for it
- You have exponentially many candidate tests and need to find a violating one using few copies.
- You need the state to remain intact for future rounds (gentle measurement is the constraint).
- Any online/adaptive learning setting where hypothesis refinement happens iteratively.

## Complexity
$O(\log^4 M / \varepsilon^2)$ copies per search call. The $\log^4$ comes from: $\log M$ binary search levels × $\log^2 M$ per Quantum-OR call × polylog error factors.

## Caveat
- Requires joint measurements across copies; not implementable with single-copy measurements.
- Gentleness is approximate: state disturbance accumulates across many iterations (bounded via quantum union bound, but non-zero).
- The gap degradation across binary search levels introduces a $\log^2 M$ overhead over a single Quantum-OR call; sometimes avoidable with better search strategies.

## Related notes
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Quantum OR Subroutine]]
- [[Gentle Measurement Lemma]]
- [[Postselected Hypothesis Update (Multiplicative Weights)]]
