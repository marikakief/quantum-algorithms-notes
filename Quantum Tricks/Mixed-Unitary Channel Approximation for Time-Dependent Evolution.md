# Mixed-Unitary Channel Approximation for Time-Dependent Evolution

> **Tags:** #trick #channel-simulation #diamond-norm #randomization
> **Source:** arXiv:1906.07115, Sections 3 and A

## What it does

Approximates the target time-ordered unitary channel $E(t,0)(\rho) = E(t,0)\rho E(t,0)^\dagger$ by a **convex mixture of short-time unitary channels**, each corresponding to a single term $H_j(\tau)$ at a sampled time $\tau$.

The mixed-unitary channel is:

$$
\mathcal{U}(t,0)(\rho) = \sum_{l=1}^L \int_0^t d\tau\, p_l(\tau)\, e^{-iH_l(\tau)/p_l(\tau)}\rho\, e^{iH_l(\tau)/p_l(\tau)},
$$

where $p_l(\tau) = \|H_l(\tau)\|/\|H\|_{\infty,1,1}$ is the time-and-term sampling distribution. This is the continuous [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] channel.

## How it approximates the target

The channel $\mathcal{U}$ and the ideal $E$ agree to first order in $s$ (both have the same first derivative at $s=0$ — this is the content of the universality Theorem 8). The diamond-norm error is therefore second-order in $\|H\|_{\infty,1,1}$, giving the $\|H\|^2_{\max,1}/r$ bound from repeated composition.

## Error metric

The paper uses diamond norm throughout for this algorithm (appropriate for channel comparison). The rescaled Dyson algorithm uses spectral norm instead. Comparing the two requires the conversion $\|U - V\|_\diamond \le 2\|U-V\|$ (Lemma 2 in the paper), losing at most a factor of 2.

## Implementation

Each "shot" of the channel is:
1. Classically sample $(\tau, l)$ from $p_l(\tau)$.
2. Apply the unitary $e^{-iH_l(\tau)/p_l(\tau)}$.
3. Repeat $r$ times for $r$ independent shots per time segment.

This is a randomized circuit: different runs use different gate sequences. The output is a mixture over runs, which approximates the ideal channel in diamond norm.

## Caveats

- This is not a single deterministic circuit; it is a randomized one. Post-selecting or coherently combining shots would require amplitude amplification.
- Quadratic scaling in $\|H\|_{\max,1}$ (vs. near-linear for rescaled Dyson) is the asymptotic cost of using this simpler approach.
- The output is only close to $E(t,0)$ on average (in expectation over the circuit randomness, measured in diamond norm).

## Related notes
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
