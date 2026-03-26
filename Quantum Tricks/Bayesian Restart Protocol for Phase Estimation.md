# Bayesian Restart Protocol for Phase Estimation

> **Source:** Wiebe and Granade, *Efficient Bayesian Phase Estimation*, arXiv:1508.00869 (2016)
> **Tags:** #trick #phase-estimation #Bayesian-inference #fault-tolerance #restart #RFPE #error-recovery

## What it does

Detects when the Bayesian posterior has failed (drifted far from the true phase) using cheap diagnostic measurements and a Bayes factor test, then resets to a wide prior and resumes estimation — without requiring external knowledge of the failure.

## The trick

Failures in [[Rejection Filtering for Lightweight Bayesian Updates|RFPE]] manifest as heavy tails: the Gaussian approximation occasionally commits to a wrong mode and concentrates there. The median error can be small while the mean is dominated by rare catastrophic events.

**Diagnostic experiment:** With probability $e^{-M/T_2}$ after each update (or periodically), perform a low-risk diagnostic:
- Set $\theta = \mu$ (align the phase offset with the current posterior mean).
- Set $M = \tau/\sigma$ with $\tau \ll 1$ (e.g., $\tau = 0.1$): use a very short evolution so the likelihood is weak.
- Measure; record outcome $E_{\text{diag}}$.

If the posterior is correct ($\phi \approx \mu$), then with phase offset $\theta = \mu$ and short $M$, the outcome $E = 1$ has probability $P(1 \mid \phi;\, \theta = \mu, M) = (1 - \cos(M(\phi - \mu)))/2 \approx M^2\sigma^2/2 \ll 1$. An outcome of 1 is therefore strong evidence the posterior has failed.

**Bayes factor formalisation:** Compute the ratio

$$\text{BF} = \frac{P(E_{\text{diag}} = 1 \mid \text{prior correct})}{P(E_{\text{diag}} = 1 \mid \text{prior wrong})}.$$

For the prior-correct hypothesis, $P(E = 1) \approx \tau^2/2$ (small). For the prior-wrong hypothesis (posterior mean is off by $\Delta \gg \sigma$), $P(E = 1) \approx 1/2$. The Bayes factor for "prior wrong" given outcome 1 is then $\sim 1/\tau^2$, which for $\tau = 0.1$ gives BF $\approx 100$ — strong evidence. Appendix D of the paper works this out in detail.

**Restart action:** If the diagnostic triggers:
1. Reset $\sigma$ to the initial prior width (flat restart).
2. If a spectral gap $\Delta$ is known (the eigenvalues of $U$ are separated), and $\sigma$ eventually shrinks below $\Delta/2$ after restarting, snap $\mu$ to the nearest known eigenvalue and resume.
3. Re-prepare the eigenstate input $|\phi\rangle$ if possible (may require re-running state preparation).

**Effect:** In simulations, adding the restart protocol reduces the mean error from $\sim 0.05$ rad to $\sim 10^{-6}$ rad by eliminating the heavy-tail failures. The diagnostic experiments add a small overhead but the cost is dominated by the information-bearing experiments.

## When to reach for it

- Any phase estimation protocol using a Gaussian posterior approximation ([[Rejection Filtering for Lightweight Bayesian Updates|RFPE]] or similar assumed-density approaches).
- When you need a bounded mean error, not just bounded median error.
- When eigenphase structure is known (e.g., energy levels of a Hamiltonian with a known gap), enabling the snap-to-eigenvalue optimisation.
- In near-term experiments where state preparation can be re-run without excessive cost.

## Complexity

- **Overhead from diagnostics:** Each diagnostic is one additional measurement, run with probability $e^{-M/T_2}$ per step. For high-fidelity hardware (long $T_2$), diagnostics are rare. For noisy hardware, they trigger more often — this is appropriate, since the posterior is more likely to fail.
- **Expected additional experiments:** $O(\log(1/\varepsilon))$ diagnostic experiments over the course of $O(\log(1/\varepsilon))$ information experiments — sublinear overhead.
- **Restart cost:** Each restart resets $\sigma$ to initial width, requiring another $O(\log(1/\varepsilon))$ experiments to re-converge. Multiple restarts multiply this cost, but failures are rare in practice.

## Caveat

- The diagnostic threshold needs tuning. Setting $\tau$ too large makes it insensitive (misses failures); too small makes it overly trigger-happy (false restarts waste budget).
- Restart requires re-preparing the eigenstate $|\phi\rangle$. If state preparation is expensive (e.g., adiabatic preparation of a ground state), the cost of restarts can be significant.
- The snap-to-eigenvalue step requires prior knowledge of the spectrum. Without this, a failed posterior can only be reset to uniform, not corrected.
- The protocol addresses occasional catastrophic failures but does not fix systematic bias from the Gaussian approximation. Gradual drift from model mismatch (e.g., unmodelled noise) reduces the decay rate $\lambda$ but doesn't trigger restarts — that's handled by the noisy likelihood model in Section III.A.

## Related notes
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]]
- [[Rejection Filtering for Lightweight Bayesian Updates]] — the update rule this monitors and protects
- [[PGH Adaptation for Single-Parameter Phase Estimation]] — experiment design used alongside this protocol
- [[Noise-Robust Bayesian Hamiltonian Learning]] — analogous graceful-degradation strategy for QHL
- [[Iterative Phase Estimation (Kitaev)]] — deterministic alternative with no failure mode of this type
- [[Sequential Monte Carlo for Quantum Parameter Estimation]] — heavier alternative with better tail behaviour (no Gaussian approximation)
