
> **Source:** Motlagh and Wiebe, arXiv:2308.01501 (2023)
> **Tags:** #trick #phase-synthesis #QSP #GQSP #numerics #polynomial-completion

## What it does

Given a polynomial $P \in \mathbb{C}[z]$ with $|P(z)|^2 \leq 1$ on the unit circle, finds a complementary polynomial $Q$ with $\deg(Q) = \deg(P)$ satisfying $|P(z)|^2 + |Q(z)|^2 = 1$ on $|z| = 1$. Once you have $(P, Q)$, you can extract the GQSP rotation angles via [[Recursive Phase Extraction from Polynomial Coefficients]].

## The trick

Write $P(z) = \sum_{k=0}^d a_k z^k$ and $Q(z) = \sum_{k=0}^d b_k z^k$. The norm condition $|P|^2 + |Q|^2 = 1$ on the unit circle expands as a condition on the autocorrelation sequences:

$$[\vec{a} \star \overline{\mathrm{rev}(\vec{a})}]_k + [\vec{b} \star \overline{\mathrm{rev}(\vec{b})}]_k = \delta_{k,d}$$

where $\star$ denotes convolution and $\mathrm{rev}(\vec{a})_k = a_{d-k}$.

**Optimisation:** Minimise

$$L(\vec{b}) = \left\| \vec{a} \star \overline{\mathrm{rev}(\vec{a})} + \vec{b} \star \overline{\mathrm{rev}(\vec{b})} - \vec{\delta}_d \right\|^2$$

over coefficients $\vec{b} \in \mathbb{C}^{d+1}$. The key: each term in the objective is an autocorrelation, which is computable via FFT in $O(d \log d)$ time. The gradient with respect to $\vec{b}$ is also an FFT convolution.

Run gradient descent (or L-BFGS) with FFT-accelerated objective and gradient evaluations. Scales to degree $\sim 10^7$ in under a minute on GPU.

## When to reach for it

- You have a target polynomial $P$ from a physics/algorithm problem (e.g., [[Hamiltonian simulation]], eigenvalue filter) and need the full GQSP circuit angles.
- The degree is too high for algebraic complementary polynomial methods (e.g., Riesz-Fejér factorisation requires finding polynomial roots, which is numerically fragile at high degree).
- You want the complementary polynomial for [[Generalized Quantum Signal Processing (GQSP)|GQSP]] without caring which specific $Q$ you get — any valid $Q$ will do.

## Complexity

- **Per iteration:** $O(d \log d)$ via FFT
- **Total:** Empirically converges in tens to hundreds of iterations for $d \sim 10^7$
- **Hardware:** GPU parallelism exploited via batch FFT; GPU time under a minute for degree $10^7$

## Caveat

Non-convex optimisation — no global convergence guarantee. Works well in practice (Motlagh-Wiebe tested extensively), but pathological $P$ close to the boundary $|P|^2 = 1$ could cause slow convergence or local minima. When $Q$ must also satisfy additional constraints (e.g., $Q$ itself is a specified function), this unconstrained approach needs modification.

If you already know $Q$ (or can derive it analytically), use [[Recursive Phase Extraction from Polynomial Coefficients]] directly instead.

## Related notes

- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]]
- [[Generalized Quantum Signal Processing (GQSP)]]
- [[Recursive Phase Extraction from Polynomial Coefficients]]
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — QSPPACK: similar role for standard real-polynomial QSP
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]] — algebraic alternative for finding $Q$ from $P$
