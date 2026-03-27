# Ryser Formula for Symmetrised Operator Products

> **Source:** Mann and Helmuth, arXiv:2004.11568 (Appendix B); Ryser (1963)
> **Tags:** #trick #trace-computation #combinatorics

## What it does
Computes the trace of a sum over all permutations of operator products — $\mathrm{Tr}\left(\sum_{\sigma \in S_n} \prod_{i=1}^n A_{\sigma(i)}\right)$ — in $O(2^n)$ time rather than $O(n!)$, using an inclusion-exclusion identity analogous to Ryser's formula for the permanent.

## The trick
The identity:

$$\sum_{\sigma \in S_n} \prod_{i=1}^n A_{\sigma(i)} = (-1)^n \sum_{A \subseteq [n]} (-1)^{|A|} \left(\sum_{i \in A} A_i\right)^n$$

For each subset $A \subseteq [n]$, form the partial sum $S_A = \sum_{i \in A} A_i$, diagonalise it (cost $d^3$ for $d$-dimensional operators), then compute $\mathrm{Tr}(S_A^n)$ as the sum of $n$th powers of eigenvalues.

Total cost: $2^n$ subsets $\times$ $O(d^3)$ diagonalisation $= O(2^n d^3)$.

## When to reach for it
- Computing polymer weights in quantum cluster expansions
- Any setting where you need the "permanent-like" sum $\sum_\sigma \prod_i A_{\sigma(i)}$ for operators
- When $n$ is small enough that $2^n$ is manageable but $n!$ is not (which is always the case for $n \geq 5$)

## Complexity
$O(2^n \cdot d^3)$ compared to $O(n! \cdot d^3)$ for the naïve approach.

## Caveat
Still exponential in $n$, so this is only useful when $n$ is bounded (e.g., polymer sizes in the truncated cluster expansion). For large $n$, you need a different approach entirely.

## Related notes
- [[Quantum Cluster Expansion for Partition Functions]]
- [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes]]
