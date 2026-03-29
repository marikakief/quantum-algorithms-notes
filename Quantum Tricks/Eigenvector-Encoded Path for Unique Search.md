# Eigenvector-Encoded Path for Unique Search

> **Source:** Montanaro, arXiv:1509.02374
> **Tags:** #trick #quantum-walk #search #tree #eigenvector

## What it does

When searching for a unique marked vertex in a tree, extracts the full root-to-target path from a single eigenvector of the quantum walk operator, enabling recursive halving instead of brute-force binary search.

## The trick

In the [[Quantum Walk on Backtracking Trees via Effective Resistance|backtracking quantum walk]], when there's a unique marked vertex $x_0$ at depth $n$, the eigenvalue-1 eigenvector is:

$$|\phi'\rangle = \frac{1}{\sqrt{2n}}\left(\sqrt{n}|r\rangle + \sum_{x \neq r,\, x \rightsquigarrow x_0} (-1)^{\ell(x)}|x\rangle\right)$$

This has weight $1/2$ on $|r\rangle$ and weight $1/(2n)$ on each of the $n$ vertices on the path from root to $x_0$. Measuring $|\phi'\rangle$ and *not* getting $|r\rangle$ yields a uniformly random vertex on the path. Use that vertex as the new root and recurse.

Approximate $|\phi'\rangle$ by running [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] on $|r\rangle$ and post-selecting on the eigenvalue-1 outcome. The approximation quality is controlled by the precision parameter; $O(\sqrt{Tn}/\delta^3)$ walk steps suffice for accuracy $\delta$.

The expected number of measurements to find $x_0$ is $O(\log n)$ — each measurement halves the remaining depth in expectation. Jensen's inequality on the recursion $S_n \leq \frac{1}{n}\sum_{i=0}^{n-1} S_i + C\sqrt{Tn}$ proves total walk steps $O(\sqrt{Tn})$.

## When to reach for it

- Unique-target search on trees (the promise is necessary)
- When you want to avoid the $O(n)$ overhead of binary search over subtrees
- Especially useful for "tall and thin" trees where $n$ is a significant fraction of $T$

## Complexity

$O(\sqrt{Tn} \log^3 n)$ walk steps total, improving the generic search complexity of $O(\sqrt{T} n^{3/2} \log n)$ by nearly a factor of $n$.

## Caveat

Requires a promise that there's exactly one marked vertex. With multiple marked vertices, the eigenvector structure breaks — there's no single clean path encoded in it. Also, the phase estimation post-selection succeeds with probability $\sim 1/2$, contributing to the logarithmic overhead.

## Related notes
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]
- [[Quantum Walk on Backtracking Trees via Effective Resistance]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
