# Near-Orthogonality of Standard-Fourier Basis Pairs

> **Source:** Aaronson & Ambainis, arXiv:1411.5729 (2015)
> **Tags:** #trick #lower-bound #query-complexity #Fourier-analysis #information-theory

## What it does
Shows that the set of standard basis vectors and their Hadamard images is nearly orthogonal (max pairwise inner product $1/\sqrt{N}$), implying each query to a Hadamard-correlated pair reveals only $O(1/\sqrt{N})$ information about the correlation structure.

## The trick

Consider the set $V$ of $2N$ vectors over $\mathbb{R}^N$:
$$V = \{e_x : x \in \{0,1\}^n\} \cup \{H e_x : x \in \{0,1\}^n\}$$
where $H$ is the $N \times N$ normalised Hadamard matrix ($H_{xy} = (-1)^{x \cdot y}/\sqrt{N}$), and $e_x$ is the standard basis vector for string $x$.

**Key property:**
- $\langle e_x, e_{x'} \rangle = \delta_{xx'}$ (standard basis vectors are orthonormal)
- $\langle He_x, He_{x'} \rangle = \langle e_x, e_{x'} \rangle = \delta_{xx'}$ (Hadamard images are also orthonormal among themselves)
- $|\langle e_x, He_{x'} \rangle| = |H_{xx'}| = 1/\sqrt{N}$ for all $x, x'$ (cross inner products between the two groups are all $1/\sqrt{N}$)

So any two vectors from *different* groups in $V$ have inner product $1/\sqrt{N}$; within each group they are orthogonal.

**Why this controls the lower bound:** In the Forrelation Gaussian distinguishing problem, the null hypothesis $H_0$ has all $2N$ coordinates independent; the alternative $H_1$ has the $N$ "g-coordinates" equal to $H$ times the "f-coordinates", so adjacent groups are correlated. When the algorithm queries coordinate $v_i$, the information it gains about the correlation structure (i.e., how much the posterior under $H_1$ deviates from $H_0$) is governed by the inner products $\langle e_i, He_j \rangle = 1/\sqrt{N}$. Each query thus shifts the distinguishing statistic by at most $O(1/\sqrt{N})$, and Azuma's inequality gives the $\tilde{\Omega}(\sqrt{N})$ lower bound.

**Generalisation — Gram–Schmidt lemma:** For $t$ queries, the Gram–Schmidt orthonormalisation of the queried vectors stays near-isometric: normalization factors $\beta_i \leq 1 + O(t/N)$ and cross inner products $|\langle v_i, w_j \rangle| \leq 1/\sqrt{N} + O(t/N)$ as long as $t \ll \sqrt{N}$. This controls the recursive application needed to push the bound from $\Omega(N^{1/4})$ to $\tilde{\Omega}(\sqrt{N})$.

## When to reach for it

- Proving lower bounds for problems that mix two dual bases (e.g., any problem where both $f(x)$ and $\hat{f}(y)$ are queried — the standard and Fourier bases simultaneously).
- Bounding the information per query in Bayesian games involving Hadamard/Fourier structure.
- Any setting where the "hard instances" lie in a subspace spanned by mixed standard-Fourier vectors and you need to show that adaptive querying is slow to detect the subspace.

## Complexity
This is a structural property, not an algorithm component. The $1/\sqrt{N}$ inner product bound follows directly from the Hadamard matrix definition.

## Caveat
- Specific to the Hadamard (Walsh-Fourier) transform over $\{0,1\}^n$. Analogous bounds hold for other unitary transforms with small $\ell_\infty$ matrix entries (e.g., DFT), but the details change.
- The near-orthogonality bounds the *per-query* contribution; bounding the *total* contribution after $T$ queries requires a martingale or Azuma argument on top of this observation.
- When $t \gtrsim \sqrt{N}$ the Gram–Schmidt control breaks down and the argument no longer gives the tight bound.

## Related notes
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]
- [[Gaussian Lifting for Query Lower Bounds]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
- [[Query Magnitude Hybrid Argument for Oracle Lower Bounds]]
- [[Coset Sampling via Fourier Transform]]
