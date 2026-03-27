# Barvinok Interpolation for Partition Function Approximation

> **Source:** Barvinok (2016, monograph); applied in Mann-Bremner, arXiv:1806.11282
> **Tags:** #trick #partition-function #FPTAS #zero-freeness #interpolation

## What it does
Converts a **zero-freeness** result for a combinatorial polynomial into an efficient multiplicative approximation algorithm, by Taylor-expanding the logarithm around an easy-to-evaluate point.

## The trick
Let $P(z) = \sum_k a_k z^k$ be a polynomial with $P(0)$ easy to compute, and suppose all roots of $P$ lie outside the disc $|z| \leq R$. Then $\log P(z)$ is analytic for $|z| < R$, and its Taylor expansion converges:

$$\log P(z) = \log P(0) + \sum_{j=1}^m c_j z^j + O(z^{m+1})$$

The coefficients $c_j$ can be expressed in terms of inverse power sums of the roots $\{r_i\}$:

$$\sum_i r_i^{-j}$$

Truncating at $m = O(\log(n/\varepsilon))$ gives $|\widehat{\log P}(z) - \log P(z)| \leq \varepsilon$ for $|z| < R$, hence a multiplicative $\varepsilon$-approximation to $P(z)$.

The key bottleneck is computing the inverse power sums. On bounded-degree graphs, the Patel-Regts technique expresses them as linear combinations of connected induced subgraph counts, computable in polynomial time.

## When to reach for it
- Approximating partition functions (Ising, graph homomorphism, Potts) on bounded-degree graphs
- Any setting where you can prove zero-freeness of a polynomial in a disc and need to convert it into an algorithm
- Classical simulation of quantum circuit amplitudes expressible as partition functions

## Complexity
$|V|^{O(1)} \cdot (1/\varepsilon)^{O(1)}$ — deterministic polynomial time. The degree of the polynomial depends on $\log(d\Delta)$ where $d$ is the local dimension and $\Delta$ the max degree.

## Caveat
Requires proving zero-freeness in a disc first — this is the hard part. The radius of the zero-free disc determines the convergence radius of the FPTAS. Outside this radius, the problem may be #P-hard.

## Related notes
- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes]]
- [[Truncated Cluster Expansion as FPTAS]] — alternative paradigm (cluster expansion vs interpolation)
- [[Quantum Cluster Expansion for Partition Functions]]
