# Near-Optimal Hardy-Space Kernel for LCHS

> **Source:** An, Liu, Lin, arXiv:2303.01029; formally developed in An, Childs, Lin, arXiv:2312.03916; used in An, Childs, Lin, arXiv:2411.04010
> **Tags:** #trick #LCHS #kernel #Hardy-space #quadrature

## What it does

Provides an explicit, near-optimal kernel function $f(k)$ for the [[LCHS Kernel for Non-Unitary Dynamics|LCHS identity]], achieving near-exponential decay in $|k|$ so that the $k$-integral truncates efficiently.

## The trick

Choose the kernel:

$$f(k) = \frac{1}{2\pi}\, e^{-2\beta}\, e^{(1+ik)^\beta}, \quad \beta \in (0,1).$$

This $f$ lives in the Hardy space $H^1(\mathbb{C}^-)$ and satisfies the LCHS normalisation $\int_\mathbb{R} f(k)/(1-ik)\, dk = 1$.

The decay is $|f(k)| \sim e^{-c|k|^\beta}$ — sub-exponential but faster than any polynomial. Truncating to $|k| \leq K$ with $K = O((\log 1/\varepsilon)^{1/\beta})$ gives error $\leq \varepsilon$.

Taking $\beta$ close to 1 makes the decay nearly exponential (but never truly exponential — there's a fundamental barrier from the Hardy-space constraint). In practice, $\beta = 0.9$ or similar gives good constants.

## When to reach for it

Whenever implementing LCHS or [[Laplace-Transform LCU Lifting for Eigenvalue Transforms|Lap-LCHS]] and you need to choose the kernel for the $k$-integral. This is the best known general-purpose choice.

## Complexity

The $k$-truncation contributes a factor of $K = O((\log 1/\varepsilon)^{1/\beta})$ to the overall query complexity. For $\beta$ close to 1, this is nearly $O(\log 1/\varepsilon)$, which is essentially free compared to the $\alpha_A T$ factor.

## Caveat

The decay exponent $\beta$ trades off against the $L^1$ norm $\|f\|_{L^1}$, which enters the block-encoding normalisation. Pushing $\beta \to 1$ improves truncation but increases the constant in $\|f\|_{L^1}$. The optimal $\beta$ depends on the application.

## Related notes

- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]] — where this kernel is formally introduced and analysed
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[Generalised LCHS Identity]]
- [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels]] — proves this decay rate is near-optimal
- [[Adaptive Kernel Parameter for LCHS]]
- [[Laplace-Transform LCU Lifting for Eigenvalue Transforms]]
