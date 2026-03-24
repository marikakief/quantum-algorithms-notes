# Variational Error Suppression

> **Source:** McClean, Romero, Babbush & Aspuru-Guzik, arXiv:1509.04279
> **Tags:** #trick #VQE #error-mitigation #NISQ #variational

## What it does

Exploits the fact that the variational principle $\langle H \rangle_\rho \geq \lambda_1$ holds for mixed states $\rho$, not just pure states. When a noisy device produces $\rho(\vec\theta)$ instead of $|\Psi(\vec\theta)\rangle\langle\Psi(\vec\theta)|$, the classical optimizer still finds parameters that minimize $\langle H \rangle$ — implicitly steering toward the parameter regime where device errors are least harmful.

## The trick

Consider a parameterized state preparation that, due to noise, produces a mixed state:

$$\rho(\vec\theta) = (1-p)\, |\Psi(\vec\theta)\rangle\langle\Psi(\vec\theta)| + p\, \rho_{\mathrm{noise}}$$

where $p$ is the error rate. The variational optimization $\min_{\vec\theta} \mathrm{Tr}[\rho(\vec\theta) H]$ still yields:

$$\mathrm{Tr}[\rho(\vec\theta) H] = (1-p)\langle\Psi(\vec\theta)|H|\Psi(\vec\theta)\rangle + p\,\mathrm{Tr}[\rho_{\mathrm{noise}} H] \geq \lambda_1$$

The minimizing $\vec\theta$ still corresponds to the best approximation of the ground state *given the noise model*. If noise is parameter-dependent (e.g., longer circuits have more error), the optimizer naturally avoids high-error regions.

For the special case of depolarizing noise $\rho_{\mathrm{noise}} = I/2^n$:

$$\langle H \rangle_\rho = (1-p)\langle H \rangle_\psi + p\,\mathrm{Tr}[H]/2^n$$

The energy is biased toward the infinite-temperature value, but the optimal $\vec\theta$ is unchanged — the landscape is compressed, not deformed.

## When to reach for it

- Near-term / [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]] implementations on noisy hardware
- When you can't characterize the noise model precisely but want some resilience
- As a theoretical justification for why VQE is noise-tolerant in principle

## Complexity

No additional quantum or classical overhead beyond standard VQE. The error suppression is automatic — it comes free from the variational principle.

## Caveat

This is a real effect but it's weaker than it sounds:

1. **Depolarizing noise compresses the landscape**, reducing the signal-to-noise ratio. For $n$-qubit systems with error rate $p$, the energy gradient scales as $(1-p)^n$, which vanishes exponentially — the same mechanism as noise-induced [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|barren plateaus]].

2. **Non-depolarizing noise** (coherent errors, correlated noise) can actually *shift* the optimal parameters away from the true ground state, not just compress the landscape.

3. The original paper demonstrates this on a single-qubit system, where everything is fine. The many-qubit scaling is where variational error suppression breaks down.

## Related notes

- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]
