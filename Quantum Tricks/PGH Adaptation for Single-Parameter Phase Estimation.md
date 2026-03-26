# PGH Adaptation for Single-Parameter Phase Estimation

> **Source:** Wiebe and Granade, *Efficient Bayesian Phase Estimation*, arXiv:1508.00869 (2016)
> **Tags:** #trick #phase-estimation #adaptive-experiments #experiment-design #Heisenberg-limit #PGH

## What it does

Reduces the [[Particle Guess Heuristic for Adaptive Quantum Metrology|Particle Guess Heuristic (PGH)]] to a closed-form rule for single-parameter phase estimation: choose the evolution length as $M = \lceil 1.25 / \sigma \rceil$ and the phase offset $\theta$ by sampling from the current prior. No particle set required.

## The trick

In the multi-parameter QHL setting, the [[Particle Guess Heuristic for Adaptive Quantum Metrology|PGH]] selects evolution time $t = 1/\|\mathbf{x}' - \mathbf{x}\|$ by sampling two particles from the posterior. For single-parameter phase estimation, the posterior is $\mathcal{N}(\mu, \sigma^2)$ and the expected inter-particle distance is $\mathbb{E}[|\phi' - \phi|] \propto \sigma$. Substituting and empirically optimising the proportionality constant gives:

$$M = \left\lceil \frac{1.25}{\sigma} \right\rceil, \qquad \theta \sim \mathcal{N}(\mu, \sigma^2).$$

For non-integer $M$ (e.g., when $U = e^{-iHt}$ with continuous $t$), use $M \leftarrow 1.25/\sigma$ directly.

**Why $\theta \sim P(\phi)$:** The phase sensitivity of the cosine likelihood $P(0 \mid \phi;\, \theta, M) = (1 + \cos(M(\phi + \theta)))/2$ is maximised when $M(\phi - \theta) \approx \pm \pi/2$. Since $\phi$ is unknown, sampling $\theta$ from the prior $P(\phi)$ achieves this on average — the expected product $M(\phi - \theta) \approx M \cdot 0 = 0$ isn't right; the idea is that $|\phi - \theta| \sim \sigma$ typically, so $M|\phi - \theta| \approx 1.25$, which is close to the sensitivity-maximising value.

**Decoherence-limited variant:** When $T_2$ decoherence is present, cap the evolution length:
$$M = \min\!\left(\left\lceil \frac{1.25}{\sigma} \right\rceil,\; T_2\right).$$

Once the posterior width $\sigma < 1.25/T_2$, experiments are capped and learning slows from exponential to polynomial — this is the correct physical behaviour. Trying to use $M > T_2$ would give uninformative experiments (likelihood collapses to $1/2$).

**The 1.25 factor:** Empirically optimised for RFPE. It represents a tradeoff: smaller constants make experiments too sensitive (likelihood wraps within the prior width), larger constants reduce information per shot. The value $1.25 < \pi$ ensures $M\sigma < \pi$, keeping the posterior unimodal and the Gaussian approximation valid for [[Rejection Filtering for Lightweight Bayesian Updates|rejection filtering]].

## When to reach for it

- Any single-parameter Bayesian phase estimation where you maintain a Gaussian posterior.
- Direct substitute for PGH when you've replaced the particle filter with a Gaussian parametrisation.
- When you need adaptive experiment design in $O(1)$ compute — no optimisation, no particle draws.
- Generalises straightforwardly to parameter-estimation problems with cosine-type likelihoods (NV-centre magnetometry, Ramsey spectroscopy, etc.).

## Complexity

- **Experiment design cost:** $O(1)$ — just compute $1.25/\sigma$ and sample from a Gaussian.
- **Information per experiment:** Near-optimal; matches Heisenberg-limited scaling empirically.
- **Comparison:** Full Bayes-optimal experiment design requires an $O(N_p)$ integral over the posterior at each step. PGH approximates this in $O(1)$.

## Caveat

- The 1.25 factor is tuned for the cosine likelihood specific to phase estimation. Different likelihood shapes (e.g., non-cosine response functions) need re-tuning.
- $\theta \sim P(\phi)$ can give poor choices when the posterior is very wide — early experiments may have high phase sensitivity but poor frequency alignment. A short warm-up phase with small fixed $M$ can stabilise early convergence.
- Multimodal posteriors break the single-sample-from-Gaussian design: $\sigma$ would be large (spanning the modes) and $M = 1.25/\sigma$ would be small, giving only coarse experiments. The [[Bayesian Restart Protocol for Phase Estimation|restart protocol]] is the safety net here.

## Related notes
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]]
- [[Particle Guess Heuristic for Adaptive Quantum Metrology]] — the parent technique for multi-parameter models
- [[Rejection Filtering for Lightweight Bayesian Updates]] — the update rule this experiment design is paired with
- [[Bayesian Restart Protocol for Phase Estimation]] — handles failures from wide-prior missteps
- [[Iterative Phase Estimation (Kitaev)]] — non-adaptive baseline that this improves upon
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — source of the original multi-parameter PGH
