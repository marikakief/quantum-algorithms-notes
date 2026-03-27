
> **Source:** Low, King, Berry et al., arXiv:2502.15882
> **Tags:** #trick #error-analysis #phase-estimation #quantum-chemistry

## What it does
Converts systematic Hamiltonian approximation errors into zero-mean random variables, so they add in quadrature with phase estimation uncertainty instead of linearly. This allows a larger $\sigma_{\text{PEA}}$ within the same total error budget, reducing the number of phase estimation repetitions.

## The trick

Standard error budget: $\sigma_E \leq \sigma_{\text{PEA}} + |\varepsilon_{\text{corr}}| \leq \varepsilon_{\text{chem}}$, where $\varepsilon_{\text{corr}}$ is the systematic error from Hamiltonian approximation (tensor factorization truncation). This forces $\sigma_{\text{PEA}}$ to be small.

**Randomized version:** Run the [[DFTHC Factorization|DFTHC]] optimization multiple times with:
1. Random initial conditions (unit vectors for $u^{(r)}_b$, small Gaussian noise for $w$)
2. Randomized bit-of-precision truncation

This makes $\varepsilon_{\text{corr}}$ a random variable with $\bar{\varepsilon}_{\text{corr}} \approx 0$ and controllable standard deviation $\sigma_{\text{corr}}$. Now errors add as:

$$\sigma_E \leq \sqrt{\sigma_{\text{PEA}}^2 + \sigma_{\text{corr}}^2} \leq \varepsilon_{\text{chem}}$$

For chemical accuracy ($\varepsilon_{\text{chem}} = 1.6$ mHa): can use $\sigma_{\text{PEA}} = 1.4$ mHa + $\sigma_{\text{corr}} = 0.774$ mHa instead of the worst-case $\sigma_{\text{PEA}} = 1.0$ mHa + $|\varepsilon_{\text{corr}}| \leq 0.6$ mHa. The 40% increase in $\sigma_{\text{PEA}}$ translates to ~30% fewer phase estimation repetitions.

## When to reach for it
- Any fault-tolerant algorithm where the Hamiltonian is approximated (tensor factorization, truncation, etc.)
- When the approximation error has no guaranteed sign but can be randomized to be unbiased
- Particularly useful when $\sigma_{\text{PEA}}$ dominates the total cost (as in qubitization-based phase estimation)

## Complexity
No additional quantum cost. Classical cost: multiple DFTHC optimizations from different initial conditions (already needed for reliability).

## Caveat
Not directly comparable to prior work that uses worst-case error budgets. The technique relies on $\varepsilon_{\text{corr}}$ being approximately unbiased when averaged over randomized initial conditions — this is empirically supported but not rigorously proven for all systems. For strongly correlated systems where the DFTHC representation changes the physics qualitatively, the random variable assumption may break down.

## Related notes
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]]
- [[DFTHC Factorization]]
- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]]
- [[Gapped Phase Estimation]]
