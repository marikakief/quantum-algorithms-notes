# Adaptive Cutoff for Recursive Quantum Circuits

> **Source:** Parent, Roetteler, Mosca, arXiv:1706.03419 (2017)
> **Tags:** #trick #circuit-optimisation #reversible-computation #recursive

## What it does

Improves the practical gate count of a recursive quantum circuit by switching from the recursive algorithm to a simpler base-case algorithm when the subproblem size drops below an empirically determined cutoff.

## The trick

Recursive algorithms like Karatsuba multiplication have asymptotically better scaling ($O(n^{1.585})$ vs. $O(n^2)$) but worse constant factors due to the overhead of decomposition, combination, and [[Reversible Computation via Compute-Copy-Uncompute|compute-copy-uncompute]] cleanup at each level. Below some crossover size, the naive algorithm is cheaper.

The optimisation has two parts:

1. **Cutoff selection:** For each subproblem size from 1 up to some maximum, evaluate the total gate count of (a) recursing further and (b) switching to the schoolbook method. Choose whichever is smaller. For Karatsuba multiplication with [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro adders]], the optimal cutoff is approximately 11 bits.

2. **Adaptive splitting:** Instead of always splitting $n$-bit inputs into two $n/2$-bit halves, use dynamic programming to find the split point at each level that minimises total cost. This matters when $n$ is not a power of 2, since unequal splits can better balance the three sub-multiplications.

Together, for $n = 400$ the adaptive Karatsuba with cutoff 11 ("aKara11") requires about 422,000 Toffoli gates, compared to 640,000 for pure schoolbook — a 34% reduction that pure asymptotic Karatsuba without cutoff tuning does not achieve at this moderate size.

## When to reach for it

- Any recursive quantum circuit where the recursion has non-trivial overhead per level (additions, copies, uncomputation). The crossover between recursive and base-case methods is rarely at subproblem size 1.
- Karatsuba or Toom-Cook multiplication circuits.
- Recursive quantum algorithms for polynomial arithmetic, FFT, or matrix multiplication where the recursive overhead includes reversible bookkeeping.
- Resource estimation: getting practical gate counts right (not just asymptotics) requires finding the correct cutoff.

## Complexity

Depends on the specific algorithms. For Karatsuba vs. schoolbook multiplication: the crossover is at $n \approx 11$ bits, giving $\sim$34% fewer Toffolis at $n = 400$ compared to schoolbook. The asymptotic scaling is unchanged at $O(n^{\log_2 3})$.

## Caveat

- The optimal cutoff depends on the specific gate costs of both the recursive and base-case algorithms. Changing the adder primitive (e.g., from [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro]] to [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper's QFT adder]]) or switching from Toffoli counting to T-counting changes the crossover point.
- The adaptive splitting adds compile-time overhead (a dynamic programming pass over all possible splits at each level), but this is a classical precomputation, not a runtime cost.
- At very large $n$, the cutoff matters less — the asymptotic term dominates. The benefit is largest at moderate sizes ($n \sim 100$-$1000$), which is exactly the regime relevant to near-term fault-tolerant quantum computing.

## Related notes
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — source paper
- [[Pebble Game Space Reduction for Recursive Circuits]] — the complementary space optimisation
- [[In-Place Adders to Minimise Karatsuba Garbage]] — reduces the per-level overhead that determines the cutoff
- [[MAJ-UMA Decomposition for Reversible Adders]] — the adder whose cost determines the optimal cutoff
