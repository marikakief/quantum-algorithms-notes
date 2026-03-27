
> **Source:** Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry, arXiv:2210.15817 (2025)
> **Tags:** #trick #product-formulas #numerical-optimization #symplectic-integrators

## What it does

Finds dramatically better [[Product Formulas]]s by using more stages than the minimum required for a given order, creating excess free parameters for numerical optimization of the error constant.

## The trick

For an order-$k$ Yoshida [[Product Formulas]] with $M = 2m+1$ stages, there are $m$ free parameters (the coefficients $w_1, \ldots, w_m$) and a fixed number of nonlinear equations from the order conditions. At 8th order, the minimum is $m=7$ (7 equations for 7 unknowns). Increasing to $m=10$ gives 10 unknowns for the same ~7 equations — 3 extra degrees of freedom.

The procedure:
1. **Random-start Levenberg-Marquardt** from $\vec{w} \sim \mathcal{N}(0, \sigma^2 I)$ with $\sigma \approx 0.6$
2. **Filter** thousands of solutions by [[Eigenvalue Error as the Correct Product Formula Metric|eigenvalue error]] on random test Hamiltonians (geometric mean over 1024 samples)
3. **Refine** via Taylor expansion: minimize the $(k+1)$-th order Taylor error to get a nearby non-solution, then re-solve for order $k$ from this improved starting point
4. **Iterate** refinement — each cycle often reduces the error further

At $m=7$ (minimal), the search finds ~600 solutions and nearly saturates. At $m=10$, over 100,000 solutions exist with huge variation in error constants — the best is 100× better than any $m=7$ or $m=8$ solution.

Critical observation: **the best solutions have no pattern in their coefficients.** Prior work (McLachlan 2002, Blanes-Casas-Murua 2006) imposed coefficient symmetries as a heuristic (e.g., setting six coefficients equal in their P198 formula). This is too restrictive. Distinct coefficients with no apparent structure give better results.

## When to reach for it

- Constructing [[Product Formulas]]s for a specific simulation: don't use minimal-length formulas. Go 2–4 stages beyond the minimum.
- The same idea applies more broadly: whenever you're solving a system of equations with a design goal (not just feasibility), add extra parameters and optimize.
- Also useful for [[Processed Product Formula Kernel-Processor Decomposition|processed formulas]], where the kernel already has fewer constraints — over-parameterization compounds the advantage.

## Complexity

The search cost is classical and one-time. Each Levenberg-Marquardt solve is fast; the expense is in testing millions of solutions on random matrices. The 8th-order results in this paper required hundreds of thousands of random starts.

## Caveat

There's no guarantee of global optimality. The solution space at $m=10$ is enormous and the error landscape is non-convex. The paper's solutions may not be the best possible — further searches could find improvements. Also, selecting by error on random Hamiltonians may not match performance on structured Hamiltonians, though the paper verifies consistency across random matrices, Ising models, and fermionic Hamiltonians.

## Related notes
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]]
- [[Processed Product Formula Kernel-Processor Decomposition]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Order-Condition Cancellation in Product Formulas]]
