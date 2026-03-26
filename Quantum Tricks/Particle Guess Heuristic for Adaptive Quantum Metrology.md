# Particle Guess Heuristic for Adaptive Quantum Metrology

> **Source:** Wiebe, Granade, Ferrie, Cory, arXiv:1309.0876 (2014)
> **Tags:** #trick #hamiltonian-learning #adaptive-experiments #experiment-design #metrology #SMC

## What it does
Adaptively selects the evolution time for each Hamiltonian-learning experiment by sampling two particles from the current posterior and setting $t = 1/\|\mathbf{x}^{-\prime} - \mathbf{x}^-\|$ — tuning sensitivity to the current posterior width without solving an expensive optimisation problem.

## The trick

At each step, sample two particles $\mathbf{x}^-$, $\mathbf{x}^{-\prime}$ uniformly (weighted by $w_j$) from the current SMC posterior. Set:
- Inversion Hamiltonian: $H^- = H(\mathbf{x}^-)$
- Evolution time: $t = 1 / \|\mathbf{x}^{-\prime} - \mathbf{x}^-\|_2$

**Why this works:** The typical inter-particle distance $\|\mathbf{x}^{-\prime} - \mathbf{x}^-\|$ scales as the posterior standard deviation $\sigma$. Setting $t \sim 1/\sigma$ means experiments are tuned to maximally discriminate among the hypotheses still under consideration — exactly what Heisenberg-limited phase estimation achieves in single-parameter settings. As the posterior concentrates, $\sigma$ decreases and $t$ grows, automatically increasing precision.

**Single-parameter analysis (closed form):** For $H(x) = x \sigma_z^{(1)}\sigma_z^{(2)}$, prior $x \sim \mathcal{N}(\mu, \sigma^2)$, the IQLE likelihood for outcome $d \in \{0,1\}$ is

$$\Pr(d | x; x^-, t) = \tfrac{1}{2}\left(1 + (1-2d)\cos[2(x-x^-)t]\right).$$

The optimal $t$ for minimising expected posterior variance is $t \approx 1/(2\sigma)$, and PGH yields $t = 1/|x^{-\prime} - x^-| \approx 1/(2\sigma)$ in expectation over the two samples.

## When to reach for it

- Adaptive Bayesian parameter estimation for Hamiltonians when the parameter space is low-dimensional ($d = O(\mathrm{poly}(n))$).
- Any multi-parameter phase estimation problem where you have a particle representation of the posterior and want fast experiment design without solving an optimisation.
- Preferred over fixed-schedule experiments when posterior width varies or is not known in advance.

## Complexity

- Cost of PGH experiment design: $O(1)$ (two random draws from particle set, one norm computation).
- Compare to full optimal experiment design: solving an optimisation over $\Pr(D|x,t)$ at each step would cost $O(N_p)$ or more; PGH replaces this with two particle draws.
- Empirical scaling: $N_{\mathrm{steps}} \approx O(d \log(1/\delta))$ experiments suffice to reach quadratic loss $\delta$ on $d$-parameter models.

## Caveat

- PGH assumes the posterior is approximately unimodal and concentrated around the true parameter. Strongly multimodal posteriors (isolated modes far apart) can cause PGH to select uninformative $t$ values.
- In the very early stages of inference (flat or wide prior), sampled particles can be far apart, giving small $t$ and correspondingly low-precision experiments. An initial warm-up phase with other experiment choices (e.g., random $t$) can help.
- The heuristic is not proven optimal; it performs near-optimally for the Ising models studied in the paper but may underperform on Hamiltonians with very different spectral structure.

## Related notes
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — robustness of PGH to noise; validated on superconducting and quantum-dot hardware
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]] — simplifies PGH to $M = \lceil 1.25/\sigma \rceil$ for single-parameter Gaussian posteriors; see also [[PGH Adaptation for Single-Parameter Phase Estimation]]
- [[PGH Adaptation for Single-Parameter Phase Estimation]] — the closed-form single-parameter variant of this trick
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]
- [[Noise-Robust Bayesian Hamiltonian Learning]]
