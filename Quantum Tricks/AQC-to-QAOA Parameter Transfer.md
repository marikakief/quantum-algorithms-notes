
> **Source:** Dong An and Lin Lin, arXiv:1909.05500
> **Tags:** #trick #QAOA #adiabatic #variational #warm-start

## What it does

Provides a principled initialisation for QAOA variational parameters by discretising an optimised AQC schedule, avoiding random initialisation and its associated barren-plateau risks.

## The trick

Given an AQC evolution $H(f(s)) = (1-f(s))H_0 + f(s)H_1$ with optimised schedule $f(s)$ and total runtime $T$, discretise into $P$ layers via first-order Trotterisation:

$$\beta_j = \frac{(1 - f(s_j)) \cdot T}{P}, \quad \gamma_j = \frac{f(s_j) \cdot T}{P}, \quad s_j = \frac{j}{P}.$$

These serve as initial QAOA angles for the ansatz $\prod_{j=1}^P e^{-i\gamma_j H_1} e^{-i\beta_j H_0} |{\psi_i}\rangle$. The adiabatic guarantee ensures these angles already produce a state close to the target (with Trotter error). Variational optimisation from this starting point can then improve the solution.

## When to reach for it

- QAOA applied to problems where a good adiabatic schedule is known (e.g., structured optimisation, linear systems, eigenpath problems).
- Situations where random QAOA initialisation leads to poor convergence or barren plateaus.
- Bridging theory and practice: the AQC-derived angles give a **provable baseline** that variational optimisation can only improve upon.

## Complexity

Same asymptotic gate complexity as the underlying AQC: the discretisation adds Trotter error $O(T^2/P)$ per layer, requiring $P = O(T^2/\varepsilon)$ for faithful reproduction. Variational refinement may allow fewer layers.

## Caveat

- The theoretical guarantee only says QAOA matches AQC — it doesn't prove QAOA beats it. Numerical evidence in the QLSP setting suggests improvement but is limited to small instances.
- The parameter transfer requires knowing the AQC schedule, which in turn requires a gap bound. For problems without analytic gap bounds, this trick doesn't directly help with initialisation.
- Classical optimisation of the $2P$ QAOA angles can itself be costly, especially at large $P$.

## Related notes
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]
- [[Gap-Adapted Adiabatic Scheduling]]
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — Shows discrete AQC with step size 1 produces QAOA angles matching optimal $O(\sqrt{N})$ for Grover search
- [[Boundary-Cancellation Schedule for Adiabatic Grover Search]]
