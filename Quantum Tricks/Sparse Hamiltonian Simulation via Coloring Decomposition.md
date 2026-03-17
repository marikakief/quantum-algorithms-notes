
> **Source:** Aharonov & Ta-Shma, arXiv:quant-ph/0301023 (2003)
> **Tags:** #trick #sparse #decomposition #simulation #historical

## What it does

The first method for simulating sparse Hamiltonians. Decomposes a row-sparse Hamiltonian into a sum of combinatorially 2×2 block-diagonal matrices using an entry-coloring scheme, then simulates each block analytically and combines via the [[Order-Condition Cancellation in Product Formulas|Trotter formula]].

## The trick

Given $H$ with at most $D$ nonzero entries per row, assign each entry $(i,j)$ a color:

$$
\mathrm{col}_H(i,j) = (k,\; i \bmod k,\; j \bmod k,\; \mathrm{rindex}_H(i,j),\; \mathrm{cindex}_H(i,j))
$$

where $k$ is the smallest integer in $[2, n^2]$ with $i \not\equiv j \pmod{k}$.

Entries sharing a color form a matrix $H_m$ that is **combinatorially 2×2 block-diagonal** — each row interacts with at most one other row. Total colors: $M = (D+1)^2 n^6$.

Each $H_m$ is a direct sum of [[1-Sparse Hamiltonian Simulation via 2×2 Blocks|1×1 and 2×2 blocks]], simulable by analytic diagonalization. Combine via [[Order-Condition Cancellation in Product Formulas|Trotter]]:

$$
\|U_\delta^{t/2\delta} - e^{-iHt}\| \leq O(M\Lambda\delta + M\Lambda^3 t\delta^2)
$$

## When to reach for it

Historically important — this is the original sparse simulation construction. In practice, use the tighter [[Edge-Coloring Decomposition for Sparse Hamiltonians|edge-coloring decomposition]] from Berry et al. (2005), which achieves $6d^2$ terms with $O(\log^* n)$ oracle queries each, or [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians|quantum walk]] methods for linear-in-$d$ scaling.

## Complexity

- Term count: $M = O(D^2 n^6)$ — much worse than later approaches
- Circuit size: $\mathrm{poly}(n, t, 1/\alpha)$
- Classical computation to determine colors (no oracle queries)

## Caveat

The $n^6$ factor in the term count is the main weakness. The coloring uses modular arithmetic and row/column indices, which is more expensive than [[Edge-Coloring Decomposition for Sparse Hamiltonians|König-based edge coloring]]. This was the first result, not the best.

## Related notes

- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]]
- [[Edge-Coloring Decomposition for Sparse Hamiltonians]] — improved decomposition
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
