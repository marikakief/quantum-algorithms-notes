
> **Source:** Costa, Schleich, Morales, Berry, arXiv:2312.09518 (Section VI.B)
> **Tags:** #trick #PDE #Carleman-linearisation #stability #discretisation #norm

## What it does
Identifies a fundamental conflict between PDE stability criteria and the rescaling needed for quantum efficiency in Carleman-based nonlinear PDE solvers, and shows that higher-order spatial discretisation is the practical mitigation.

## The trick

For a reaction-diffusion PDE $\partial_t u = D\Delta u + cu + bu^M$, stability is naturally in the max-norm:

$$\frac{\|u_{\mathrm{in}}\|_{\max}^{M-1} |b|}{|c|} < 1.$$

But the [[Carleman Vector Rescaling for Measurement Probability Boost|Carleman rescaling]] needs $R < 1$ in the 2-norm:

$$R = \frac{\|F_M\| \cdot \|\mathbf{u}_{\mathrm{in}}\|_2^{M-1}}{|\lambda_0|} < 1.$$

For a discretised PDE on $n$ grid points, $\|\mathbf{u}_{\mathrm{in}}\|_2 \propto \sqrt{n} \cdot \|\mathbf{u}_{\mathrm{in}}\|_{\max}$. So even when the PDE is perfectly stable in the max-norm, increasing $n$ inflates $R$ and eventually breaks $R < 1$.

**Resolution:** Minimise $n$ by using higher-order finite differences (order $k$, stencil size $2k+1$). The spatial discretisation error scales as $O(n^{-(2k-1)/d})$, so the number of grid points needed for error $\varepsilon$ is

$$n = \Omega\!\left(\varepsilon^{-2d/(2(2k-1)+d)}\right).$$

Larger $k$ means fewer grid points, smaller $\|\mathbf{u}_{\mathrm{in}}\|_2$, and a better chance of satisfying $R < 1$.

## When to reach for it

- Any quantum algorithm for nonlinear PDEs via Carleman linearisation
- Situations where a spatial discretisation produces a large ODE system and you're worried about the condition number or stability of the resulting quantum algorithm
- Deciding the spatial discretisation order for a quantum PDE solver: this analysis tells you that higher order is not just about accuracy — it's about *feasibility*

## Complexity

No direct cost. This is an analysis tool: it tells you whether the quantum algorithm can be efficient for a given PDE, and how to choose discretisation parameters to make it work.

## Caveat

- Higher-order finite differences ($k > 1$) break the $\|\cdot\|_\infty$-contractivity of the discretised Laplacian. Numerically, $\|e^{tDL_k}\|_\infty$ stays close to 1, but this isn't proven rigorously. The error analysis for $k > 1$ is therefore partially heuristic.
- The 2-norm condition $R < 1$ is genuinely stronger than the max-norm PDE stability criterion. There exist stable PDEs for which no finite discretisation satisfies $R < 1$. In those cases, Carleman linearisation on a quantum computer simply cannot provide an efficient solution with current methods.
- The tension is intrinsic to the Carleman approach, not to quantum computing per se. A quantum ODE solver that doesn't use Carleman linearisation (e.g., direct simulation of the nonlinear dynamics if such a method existed) wouldn't face this issue.

## Related notes
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
- [[Carleman Vector Rescaling for Measurement Probability Boost]]
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]]
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]] — alternative PDE approach (linear case) via spectral methods
