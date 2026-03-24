# Sequential Monte Carlo for Quantum Parameter Estimation

> **Source:** Wiebe, Granade, Ferrie, Cory, arXiv:1309.0876 (2014); see also Granade, Ferrie, Wiebe, Cory, arXiv:1207.1655 (2012)
> **Tags:** #trick #bayesian-inference #SMC #hamiltonian-learning #posterior #parameter-estimation

## What it does
Represents and updates a Bayesian posterior over Hamiltonian parameters using a weighted particle approximation (Sequential Monte Carlo), achieving exponential convergence in the number of experiments with $O(N_p)$ cost per update.

## The trick

**Representation:** Approximate $\Pr(\mathbf{x} | \mathrm{data})$ as

$$\Pr(\mathbf{x}) \approx \sum_{j=1}^{N_p} w_j\, \delta(\mathbf{x} - \mathbf{x}_j)$$

with $\sum_j w_j = 1$. This is a particle filter with $N_p$ hypothesis particles.

**Bayesian update on observing outcome $D$:**
$$w_j \leftarrow \Pr(D | \mathbf{x}_j)\, w_j, \quad \text{normalise to } \sum w_j = 1.$$
Cost: $O(N_p)$ likelihood evaluations, each one a call to a quantum simulator.

**Liu–West resampling** (when $N_{\mathrm{eff}} = 1/\sum_j w_j^2 < N_p/2$):

1. Resample $N_p$ particles with replacement according to weights $\{w_j\}$.
2. Jitter each particle to maintain particle diversity:
$$\mathbf{x}_j \leftarrow a\, \mathbf{x}_j + (1-a)\bar{\mathbf{x}} + \boldsymbol{\eta}, \quad \boldsymbol{\eta} \sim \mathcal{N}(0, (1-a^2)\hat{\Sigma})$$
where $\bar{\mathbf{x}} = \sum_k w_k \mathbf{x}_k$, $\hat{\Sigma} = \sum_k w_k (\mathbf{x}_k - \bar{\mathbf{x}})(\mathbf{x}_k - \bar{\mathbf{x}})^\top$, and $a \approx 0.9$.
3. Reset weights to $1/N_p$.

The posterior mean estimate is $\hat{H} = \sum_j w_j H(\mathbf{x}_j)$; posterior covariance $\hat{\Sigma}$ gives the certified credible ellipse.

**Why exponential convergence:** Each informative experiment provides $\Theta(1)$ bits of information (when likelihoods are $O(1)$, ensured by [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]]). With $d$ parameters, each experiment reduces the posterior volume by a constant factor, giving $O(d \log 1/\delta)$ experiments total.

## When to reach for it

- Bayesian parameter estimation for any quantum system where you can evaluate likelihoods (exactly or approximately).
- Hamiltonian learning with adaptive experiments (combine with [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH]] for experiment selection and [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation|IQLE]] for likelihood evaluation).
- Any problem where the posterior must be maintained online (one experiment at a time) and the parameter space is low-to-moderate dimensional.
- Quantum metrology, gate characterisation, Hamiltonian identification.

## Complexity

- Per-update cost: $O(N_p)$ likelihood evaluations + $O(N_p)$ resampling operations.
- Total experiments: $O(d \log 1/\delta)$ (empirically; consistent with $\Omega(d \log 1/\delta)$ information-theoretic lower bound).
- Particle count $N_p$: theory (via SMC convergence results) requires $N_p$ sub-exponential in $d$; in practice $N_p \sim 10^3$–$10^4$ suffices for $d \lesssim 50$ parameters.

## Caveat

- **Particle impoverishment:** if resampling is not applied (or $N_p$ is too small), particles collapse to a single point and the posterior representation fails. Liu–West jittering mitigates this but introduces a small bias (smoothing).
- **Multimodal posteriors:** if the posterior has multiple separated modes, SMC with a unimodal particle cloud can miss modes entirely. Particle counts must be large enough that at least some particles land near each mode initially.
- **Likelihood evaluation cost:** each update costs $O(N_p)$ quantum-simulator calls. For large $N_p$ and complex likelihoods, this dominates. IQLE keeps per-call cost to $O(1/\varepsilon^2)$ samples when likelihoods are $\Theta(1)$.
- **Not model-selection:** SMC infers parameters within a fixed model family $H(\mathbf{x})$. If the true Hamiltonian lies outside the family, the posterior concentrates on the nearest-in-distribution model, which may be incorrect.

## Related notes
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — SMC with noisy IQLE; model selection via marginal likelihoods
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]]
- [[Bayesian Model Selection via SMC Marginal Likelihoods]]
- [[Noise-Robust Bayesian Hamiltonian Learning]]
