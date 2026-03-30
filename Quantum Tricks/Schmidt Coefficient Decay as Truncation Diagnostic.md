# Schmidt Coefficient Decay as Truncation Diagnostic

> **Source:** Vidal, arXiv:quant-ph/0310089
> **Tags:** #trick #tensor-networks #MPS #entanglement #classical-simulation

## What it does
Uses the rate of decay of Schmidt coefficients $\lambda_\alpha^{[l]}$ to predict whether an MPS truncation will be accurate, and to set the required bond dimension $\chi_\epsilon$.

## The trick
For a bipartition of a 1D system at bond $l$, the truncation error from keeping only $\chi_\epsilon$ Schmidt values is:

$$\epsilon_1 = \sum_{\alpha = \chi_\epsilon + 1}^{\chi} |\lambda_\alpha^{[l]}|^2$$

Vidal's empirical observation: for ground states and low-energy dynamics of gapped 1D Hamiltonians with short-range interactions, the Schmidt coefficients decay roughly exponentially:

$$\lambda_\alpha^{[l]} \sim e^{-c\alpha}$$

for some constant $c > 0$. This means the truncation error $\epsilon_1$ is exponentially small in $\chi_\epsilon$ — a modest bond dimension captures the state to high accuracy.

The diagnostic: if you plot the Schmidt spectrum during a simulation and see rapid exponential decay, TEBD/DMRG will work well. If the decay slows or flattens (e.g., at a critical point or during a quantum quench), the required $\chi_\epsilon$ is growing and you're approaching the limits of MPS methods.

## When to reach for it
- Monitoring convergence of TEBD or DMRG: check whether the Schmidt tail is decaying fast enough
- Choosing $\chi_\epsilon$ adaptively: set a tolerance $\epsilon_1$ and truncate at the $\alpha$ where the cumulative tail drops below it
- Predicting when MPS methods will fail: slow decay signals high entanglement, pointing to critical systems or non-equilibrium dynamics where MPS is inefficient

## Complexity
No additional cost — the Schmidt coefficients are already computed as part of the [[Local Gate Update on MPS via Schmidt Redecomposition|MPS update]].

## Caveat
The exponential decay is an empirical observation for gapped 1D systems, not a theorem. At quantum critical points, the entanglement entropy scales as $\frac{c}{6}\log n$ (Calabrese-Cardy), and the Schmidt coefficients follow a power law rather than exponential decay. In quench dynamics, entanglement can grow linearly in time, making the required $\chi_\epsilon$ exponentially large at late times.

## Related notes
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]]
- [[Local Gate Update on MPS via Schmidt Redecomposition]]
- [[Entanglement as Simulation Complexity Measure]]
- [[MPS Canonical Form for Efficient State Tracking]]
