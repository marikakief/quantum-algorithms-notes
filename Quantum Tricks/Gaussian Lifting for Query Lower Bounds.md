# Gaussian Lifting for Query Lower Bounds

> **Source:** Aaronson & Ambainis, arXiv:1411.5729 (2015)
> **Tags:** #trick #lower-bound #query-complexity #Fourier-analysis #boolean-functions

## What it does
Transfers classical query lower bounds from continuous Gaussian inputs to discrete $\pm 1$ Boolean inputs, bypassing the combinatorial difficulty of working directly with Boolean functions.

## The trick

**Phase 1 — Gaussian problem.** Replace the Boolean input functions $f, g : \{0,1\}^n \to \{-1,+1\}$ with continuous analogues $f, g : \{0,1\}^n \to \mathbb{R}$ with each coordinate drawn i.i.d. from $\mathcal{N}(0,1)$ (null hypothesis $H_0$) or from a structured Gaussian distribution (alternative $H_1$ — in the Forrelation case, $f \sim \mathcal{N}(0, I_N)$ and $g = Hf$ for the normalised Hadamard $H$).

**Phase 2 — Prove the lower bound over Gaussians.** In the Gaussian setting the relevant quantities (covariance structure, Bayesian posteriors, information per query) are governed by standard linear algebra. Under $H_1$, the pairwise covariance between any two entries is bounded by $\varepsilon$, and an Azuma-type martingale argument bounds how fast the posterior can diverge from $H_0$. The lower bound takes the form $\tilde{\Omega}(1/\varepsilon)$ queries.

**Phase 3 — Round back to Boolean.** To produce a Boolean hard distribution, draw a Gaussian instance from $H_1$ and round each coordinate by $\text{sgn}(\cdot)$. The key claim (Theorem 9 in the paper): rounding preserves the forrelation value up to concentration bounds. Specifically, if $f, g$ are Gaussian forrelated, then after rounding the expected forrelation is $\frac{2}{\pi}\Phi_\text{Gaussian} + O(\log N / \sqrt{N})$, which is enough to maintain the promise gap for $N$ large. A classical algorithm distinguishing the Boolean instances can be used (after appropriate modification) to distinguish the Gaussian instances, so the Gaussian lower bound transfers.

## When to reach for it

- Any time you want a randomised query lower bound for a problem defined via Fourier-analytic structure over Boolean functions.
- When the problem's hard distribution is naturally described as a Gaussian process (e.g., Fourier correlation problems, problems defined by linear algebra over the input).
- When direct combinatorial arguments for Boolean inputs are messy but the same structure is clean for Gaussians.

## Complexity
No overhead — it's a proof technique, not an algorithm. The rounding step relies on standard concentration of measure (Gaussian → Bernoulli rounding); the covariance deviation is $O(1/N)$ which is negligible.

## Caveat
- Only transfers lower bounds from Gaussian to Boolean, not upper bounds or algorithms.
- Requires that rounding the Gaussian hard instance still falls in the promise. If the promise gap is too small, rounding may destroy it — you need the Boolean forrelation to concentrate tightly around the Gaussian forrelation.
- Specific to problems with a Fourier-analytic formulation. Doesn't apply to problems with no natural Gaussian analogue.

## Related notes
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]
- [[Near-Orthogonality of Standard-Fourier Basis Pairs]]
- [[Query Magnitude Hybrid Argument for Oracle Lower Bounds]]
