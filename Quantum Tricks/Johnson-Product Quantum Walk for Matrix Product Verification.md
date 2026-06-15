# Johnson-Product Quantum Walk for Matrix Product Verification

> **Source:** Buhrman-Špalek, arXiv:quant-ph/0409035
> **Tags:** #trick #quantum-walks #matrix-verification #query-complexity

## What it does

Turns a wrong matrix entry into a marked vertex of a quantum walk by sampling both its row and its column.

## The trick

For matrix product verification, do not search directly over entries $(i,j)$. Instead walk over pairs of subsets

$$
(R,S)\in {[n]\choose k}\times {[n]\choose k},
$$

using the graph

$$
G=J(n,k)\times J(n,k),
$$

where a walk step exchanges one element of $R$ and one element of $S$. Mark $(R,S)$ when the induced submatrix contains an error:

$$
W\cap(R\times S)\ne\varnothing,
$$

where $W=\{(i,j):(AB-C)_{i,j}\ne 0\}$.

For one wrong entry, the marked fraction is

$$
\epsilon=\Theta(k^2/n^2),
$$

and the spectral gap of $J(n,k)\times J(n,k)$ is

$$
\delta=\Theta(1/k).
$$

A [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy walk]] detects a marked vertex in

$$
O\left(\frac{1}{\sqrt{\delta\epsilon}}\right)=O(n/\sqrt{k})
$$

steps. Balancing this against $k$ gives $k=n^{2/3}$ and hence the $O(n^{5/3})$ verification time once each step costs $O(n)$.

## When to reach for it

Use this when a witness is indexed by a pair $(i,j)$ but the natural update structure is row-subset/column-subset based. The same pattern can apply to other bilinear or bipartite verification problems where a local defect is exposed by choosing both endpoints.

## Complexity

For matrix product verification, the walk setup costs $O(kn)$ and each update/phase step costs $O(n)$ when paired with [[Random Bilinear Compression for Submatrix Product Tests]]. With $k=n^{2/3}$ this gives $O(n^{5/3})$ worst-case time.

## Caveat

The walk only helps if the markedness test and update are cheaper than loading the whole $k\times k$ subproblem. A direct submatrix-product check would reintroduce an $\Omega(kn)$ per-step cost and erase the time speedup.

## Related notes

- [[Quantum Verification of Matrix Products (Buhrman-Špalek 2005) — Paper Notes]]
- [[Random Bilinear Compression for Submatrix Product Tests]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
