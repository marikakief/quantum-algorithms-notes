
> **Source:** Wiebe and Granade, *Efficient Bayesian Phase Estimation*, arXiv:1508.00869 (2016)
> **Tags:** #trick #Bayesian-inference #rejection-sampling #phase-estimation #particle-filter #RFPE

## What it does

Approximates a Bayesian posterior update using rejection sampling from a parametric prior (Gaussian), bypassing the full particle-filter update. Memory cost drops from $O(N_p)$ particles to $O(1)$: only the current Gaussian parameters $(\mu, \sigma^2)$ need to be stored.

## The trick

Maintain the posterior as a Gaussian $\mathcal{N}(\mu, \sigma^2)$. After observing outcome $E \in \{0, 1\}$ from an experiment with parameters $(M, \theta)$:

1. Draw $m$ samples $\phi_1, \ldots, \phi_m \sim \mathcal{N}(\mu, \sigma^2)$.
2. Accept each $\phi_j$ into $\Phi_{\text{accept}}$ with probability $P(E \mid \phi_j;\, \theta, M) / \kappa_E$, where $\kappa_E \in (0,1]$ is chosen so that $P(E \mid \phi_j;\, \theta, M) / \kappa_E \leq 1$ for all $\phi_j$.
3. Update: $\mu \leftarrow \mathbb{E}[\Phi_{\text{accept}}]$, $\sigma \leftarrow \sqrt{\mathrm{Var}[\Phi_{\text{accept}}]}$.

**Why it works:** By rejection sampling, the accepted samples are drawn from a distribution with density $\propto P(E \mid \phi;\, \theta, M) \cdot \mathcal{N}(\phi \mid \mu, \sigma^2)$. By Bayes' rule, this is proportional to the posterior $P(\phi \mid E;\, \theta, M)$. Fitting a Gaussian to these samples is an approximation — the posterior is not Gaussian in general — but it's justified when:
- The prior is already concentrated (late in inference), so the posterior is approximately unimodal.
- The likelihood is a cosine envelope over a width comparable to $\sigma$, which has approximately Gaussian cross-sections.

The Gaussian refitting is equivalent to the **assumed density filtering** approximation from classical Bayesian filtering.

**When the Gaussian fit is accurate:** When $M \sigma \lesssim \pi$ (i.e., the prior width is less than one half-period of the cosine likelihood), the posterior is unimodal and the Gaussian approximation is reasonable. The [[PGH Adaptation for Single-Parameter Phase Estimation|PGH experiment design]] enforces $M \approx 1.25/\sigma$, so $M\sigma \approx 1.25 < \pi$ — the condition is automatically maintained.

**Stability requirement:** When the likelihood is nearly flat across samples ($P(E \mid \phi_j) \approx \alpha$ with variation $|\delta_j| \leq \delta$), the number of accepted samples is small and the Gaussian fit is noisy. A sufficient condition for stability is $m = \Omega(\alpha^2/\delta^2)$. In practice $m \sim 10^3$–$10^4$ works well under [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH]]-selected experiments.

## When to reach for it

- Single-parameter phase estimation with memory or compute constraints (FPGA control hardware, embedded controllers).
- Any Bayesian estimation problem where the likelihood is approximately symmetric around the true value and the prior is roughly Gaussian.
- As a drop-in replacement for [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]] when the parameter space is one-dimensional and you want reduced memory overhead.
- Not suitable for multimodal posteriors or high-dimensional parameter spaces — [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]] handles those cases.

## Complexity

- **Memory:** $O(m)$ during each update (only two floats stored between updates). Factor of $10^3$–$10^4$ reduction over SMC with $N_p$ particles.
- **Per-update cost:** $O(m)$ likelihood evaluations. For PE, each evaluation is $O(1)$ arithmetic.
- **Experiment count:** $O(\log(1/\varepsilon))$ experiments for phase precision $\varepsilon$ (matches [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC-based PE]]).
- **Total evolution time:** $O(1/\varepsilon)$, saturating the Heisenberg limit.

## Caveat

- The Gaussian approximation breaks down for multimodal posteriors. In PE this arises from the periodicity of $\phi$ if the prior wraps around. The paper handles this with a circular statistics workaround (compute means in two frames offset by $\pi$, pick the one with smaller variance).
- Tail behaviour is worse than SMC: the Gaussian fit can miss outlier particles, leading to occasional large errors. The [[Bayesian Restart Protocol for Phase Estimation|restart protocol]] mitigates this.
- The empirical exponential convergence rate $\lambda \approx 0.17$ has no rigorous proof. The Heisenberg scaling is numerically demonstrated, not analytically derived.

## Related notes
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]]
- [[PGH Adaptation for Single-Parameter Phase Estimation]]
- [[Bayesian Restart Protocol for Phase Estimation]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]] — the heavier alternative
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]] — experiment design that keeps the likelihood informative (maintains stability condition)
- [[Noise-Robust Bayesian Hamiltonian Learning]]
