# Recursive Subproblem Amplification

> **Source:** Buhrman, Dürr, Heiligman, Høyer, Magniez, Santha, de Wolf, arXiv:quant-ph/0007016 (2001)
> **Tags:** #trick #amplitude-amplification #recursive #ordered-search #query-complexity

## What it does

Removes logarithmic factors from quantum search algorithms by recursively decomposing a search over ordered data into subproblems and amplifying over them, replacing $\log N$ with the iterated logarithm $c^{\log^* N}$ — a function so slow it's essentially constant for all practical inputs.

## The trick

Given a search problem over ordered functions $f, g: [N] \to \mathbb{Z}$ (both monotone):

1. **Divide:** Split the domain into $O(N/r)$ blocks of size $r$
2. **Align:** For each $f$-block, use binary search ($O(\log N)$ comparisons) to find the matching $g$-segment
3. **Recurse:** Solve each size-$r$ subproblem recursively
4. **Amplify:** Use [[Standard Amplitude Amplification|amplitude amplification]] over all $O(N/r)$ subproblems — costs $O(\sqrt{N/r})$ repetitions

This gives the recurrence:

$$T(N) = O\left(\sqrt{\frac{N}{r}} \cdot [\log N + T(r)]\right)$$

Choosing $r = \lceil \log^2 N \rceil$ ensures $T(r)$ dominates $\log N$, and the recursion depth is $O(\log^* N)$ (number of times you apply $\log$ before reaching $\leq 1$). Unfolding gives $T(N) = O(\sqrt{N} \cdot c^{\log^* N})$ for a constant $c$.

## When to reach for it

- Quantum search problems where the data has monotone structure that enables cheap alignment between subproblems
- Situations where you have a $O(\sqrt{N} \log N)$ algorithm and want to shave the log factor
- Any setting where the search space naturally decomposes into independently solvable blocks connected by an ordering

## Complexity

$O(\sqrt{N} \cdot c^{\log^* N})$ queries. For all practical $N$ (say $N < 2^{2^{2^{2^{2}}}}$), $\log^* N \leq 5$, so $c^{\log^* N}$ is a small constant.

## Caveat

Requires monotone structure — both functions must be ordered. Doesn't apply to general unstructured search. The recursive construction also adds implementation complexity that may not be worth the near-constant improvement in practice.

The technique is a neat theoretical trick but rarely cited in practice because (a) the log factor it removes is small and (b) for most applications, other algorithmic improvements (e.g., quantum walks) give better asymptotics anyway.

## Related notes

- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]
