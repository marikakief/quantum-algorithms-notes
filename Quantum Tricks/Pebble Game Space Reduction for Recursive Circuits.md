# Pebble Game Space Reduction for Recursive Circuits

> **Source:** Parent, Roetteler, Mosca, arXiv:1706.03419 (2017); builds on Bennett (1989) pebble games
> **Tags:** #trick #pebble-games #reversible-computation #space-time-tradeoff #recursive

## What it does

Reduces the ancilla (space) cost of a reversible recursive circuit from $O(n^{\log_b a})$ to $O(n^{1 + \frac{\log_b a - 1}{2 - \log_a b}})$, where the recursion splits into $a$ subproblems of size $n/b$ with $a > b$. The gate count stays the same; the depth increases.

## The trick

A recursive algorithm with $a$ subproblems of size $n/b$ defines a complete $a$-ary tree of depth $\log_b n$. In the naive reversible implementation, all intermediate results at every level are live simultaneously, requiring space proportional to the total gate count.

**The pebble game model:** Each tree node represents a subproblem result. A node can be computed (pebbled) only when all its children are pebbled. A node can be uncomputed (unpebbled) under the same condition. Each pebble at depth $d$ costs $O(n/b^d)$ qubits.

**The balancing strategy:**

1. Find a level $k$ where the cost of all nodes above level $k$ roughly equals the cost of each subtree below level $k$. This gives $k \leq \frac{\log_b n}{2 - \log_a b}$.
2. Process each level-$k$ subtree sequentially: compute it fully, copy its root result to a clean register via [[Reversible Computation via Compute-Copy-Uncompute|compute-copy-uncompute]], then erase the entire subtree before starting the next sibling.
3. After all level-$k$ roots are computed, handle the remaining (smaller) upper tree.

Because each subtree is erased before the next is computed, only one subtree's worth of ancillae is live at a time, plus the $O(3^k)$ copied root results.

For Karatsuba multiplication ($a = 3$, $b = 2$), $k \approx 0.731 \log_2 n$, yielding space $O(n^{1.427})$ instead of $O(n^{1.585})$.

## When to reach for it

- Any quantum algorithm built on a recursive structure where $a > b$ (i.e., the number of subproblems exceeds the size reduction factor). This includes Karatsuba multiplication, Strassen matrix multiplication, and recursive FFT variants.
- When qubit count is the binding resource constraint and you can tolerate increased depth.
- When the recursion tree is a complete $k$-ary tree (the analysis is cleanest here).

## Complexity

Space: $O(n^{1 + \frac{\log_b a - 1}{2 - \log_a b}})$ qubits. For Karatsuba ($a=3, b=2$): $O(n^{1.427})$.

Gate count: unchanged at $O(n^{\log_b a})$ (up to a constant factor $< 2\times$).

Depth: increases from the parallel baseline. For Karatsuba: $O(n^{1.158})$ (up from $O(n)$).

Volume (depth $\times$ space): unchanged at $O(n^{1 + \log_b a})$.

## Caveat

- The strategy is heuristic, not provably optimal. Better pebbling strategies for complete ternary trees are not known. Whether polynomial-step pebbling with $O(\log n)$ pebbles is possible (as for line graphs via the Lange-McKenzie-Tapp method) remains open.
- The depth increase may be unacceptable when circuit execution time is the bottleneck. The trade-off is pure space-for-depth.
- Only applies when $a > b$. If $a \leq b$ (e.g., binary search, $a = 1, b = 2$), the tree's total size is dominated by the root level, and the naive approach already has low space.

## Related notes
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — source paper
- [[Reversible Computation via Compute-Copy-Uncompute]] — the underlying cleanup primitive used at each subtree
- [[Adaptive Cutoff for Recursive Quantum Circuits]] — complementary optimisation for the base case
- [[In-Place Adders to Minimise Karatsuba Garbage]] — reduces the per-level garbage that the pebble game must manage
