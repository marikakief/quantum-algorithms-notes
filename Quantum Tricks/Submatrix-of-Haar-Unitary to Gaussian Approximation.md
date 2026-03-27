
> **Source:** Aaronson & Arkhipov, arXiv:1011.3245 (Section 5)
> **Tags:** #trick #random-matrices #linear-optics #BosonSampling #Haar-measure

## What it does

Approximates an $n \times n$ submatrix of an $m \times m$ Haar-random unitary by a matrix of i.i.d. complex Gaussians, when $n \ll m$.

## The trick

Let $U \sim \text{Haar}(m)$ and let $A = U_{[n],[n]}$ be the top-left $n \times n$ submatrix (or any fixed $n \times n$ submatrix). When $n \ll m$, the matrix $\sqrt{m} \cdot A$ converges in distribution to an i.i.d. $\mathcal{N}(0,1)_{\mathbb{C}}$ matrix. More precisely:

$$\sqrt{m} \cdot A \xrightarrow{d} \mathcal{N}(0,1)_{n \times n}^{\mathbb{C}} \quad \text{as } m \to \infty \text{ with } n \text{ fixed}$$

For finite $m$ and $n$, the total variation distance between the distribution of $\sqrt{m} \cdot A$ and $\mathcal{N}(0,1)_{n \times n}^{\mathbb{C}}$ can be bounded. Aaronson-Arkhipov need this bound to be useful for their reduction, which requires $n \leq m^{1/6}$ (approximately) for the TV distance to be small enough.

Intuitively: the columns of a Haar-random unitary are uniformly distributed on the unit sphere in $\mathbb{C}^m$. When $n \ll m$, a random vector on the unit sphere in $\mathbb{C}^m$ looks like a Gaussian vector scaled to have norm 1, and the first $n$ coordinates look essentially i.i.d. Gaussian.

This is a classical result in random matrix theory, related to the moment-matching between uniform spherical measure and Gaussian measure on low-dimensional projections.

## When to reach for it

- Connecting a quantum sampling model (linear optics) to a classical algebraic object (permanents of Gaussian matrices).
- Any argument where you need to argue that a random quantum circuit's output statistics are indistinguishable from a Gaussian ensemble for the purposes of a reduction.
- Random circuit sampling hardness arguments use analogous (but different) concentration results.
- Approximate designs and unitary $t$-designs: related reasoning shows that Haar-random circuits look like Gaussian ensembles for low-degree polynomial functions.

## Complexity

The approximation is quantitative: TV distance $\leq O(n^2/m)$ roughly (exact constants depend on the version used). No computational cost — this is a distributional statement, not an algorithm.

## Caveat

- Requires $n = o(m^{1/2})$ for the approximation to be useful; the BosonSampling reduction needs the stronger $n \leq m^{1/6}$ condition.
- This is a statement about Haar-random $U$, not about a specific $U$. For the BosonSampling reduction, $U$ is chosen randomly, which is fine — but it means the hard instances are typical random unitaries, not adversarial ones.
- The result controls the joint distribution of entries in a single submatrix; for the BosonSampling reduction you need it to hold approximately for many submatrices simultaneously (since the sampler can query multiple outputs), which requires a union-bound argument.

## Related notes
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]]
- [[Gaussian Permanent Hiding via Haar Random Embedding]]
- [[Permanent Formula for Bosonic Amplitudes]]
