
> **Source:** Tazhigulov, Sun, Haghshenas, Zhai, Tan, Rubin, Babbush, Minnich, Chan, arXiv:2203.15291
> **Tags:** #trick #error-mitigation #postprocessing #NISQ #rescaling #noise-model

## What it does

Corrects amplitude degradation in noisy quantum simulations by deriving time-dependent rescaling factors from a classically solvable reference problem run through the same noisy circuit.

## The trick

Construct a commuting sub-Hamiltonian $\hat{H}'$ from $\hat{H}$ by retaining only mutually commuting terms (e.g., for a hexagonal spin model, keep only terms on non-overlapping pairs like (1-2), (3-4), (5-6)). Because $\hat{H}'$ has only commuting terms, its dynamics are classically simulable exactly.

Run the same circuit recompilation ansatz for $\hat{H}'$ on the quantum device — the brickwork circuit still couples all qubits, so the noise profile closely matches the full-[[Hamiltonian simulation]]. For each time point $t$, compute:

$$f(t) = \frac{\langle A \rangle_t^{\mathrm{ideal}}(\hat{H}')}{\langle A \rangle_t^{\mathrm{hw}}(\hat{H}')}$$

Then rescale the full-Hamiltonian hardware results:

$$\langle A \rangle_t^{\mathrm{corrected}}(\hat{H}) = f(t) \cdot \langle A \rangle_t^{\mathrm{hw}}(\hat{H})$$

The key insight: even though $\hat{H}'$ is trivial, the circuit used to simulate it on hardware is non-trivial (same brickwork structure, same gate types, same qubit connectivity), so it samples a similar noise distribution.

## When to reach for it

- NISQ simulations where the circuit structure is fixed but the Hamiltonian can be varied.
- When the primary noise effect is amplitude suppression (depolarization-like) rather than frequency shifts.
- When you have classical access to the exact answer for a related problem — which is always the case if you can construct a commuting sub-Hamiltonian.
- Dynamical correlation functions where the spectral peaks have correct frequencies but degraded amplitudes.

## Complexity

- **Extra quantum cost:** Roughly doubles the circuit executions — you run the same circuits for $\hat{H}'$ as for $\hat{H}$.
- **Classical cost:** Exact simulation of $\hat{H}'$ is efficient (commuting terms → product of 2-qubit problems).
- **No additional circuit depth** — same ansatz, different target unitary parameters.

## Caveat

- Assumes the noise profile of $\hat{H}'$ circuits matches $\hat{H}$ circuits. This holds when both use the same ansatz structure (same gate count, same connectivity), but breaks down if the optimal circuit parameters for $\hat{H}$ happen to hit particularly noisy operating points.
- Only corrects multiplicative errors (amplitude suppression). Does not fix frequency shifts, which would require additive corrections.
- Doesn't help when the data is completely degraded — in the 10-site $\alpha$-RuCl$_3$ experiment (310 two-qubit gates), the signal was too destroyed for rescaling to recover anything.
- Requires choosing $\hat{H}'$ carefully: it must be commuting (for classical solvability), but its circuit recompilation must produce a similar noise environment. Picking terms that are too simple may not capture the right noise structure.
- Related to but distinct from zero-noise extrapolation: this rescales using a reference problem rather than extrapolating from amplified noise.

## Related notes
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]]
- [[Number-Basis Postselection for Error Mitigation]] — complementary technique (symmetry-based filtering vs. reference-based rescaling)
- [[Symmetry-Based QSE for Unencoded Error Mitigation]] — another approach to NISQ error mitigation
- [[Stabilizer Subspace Projection for Error Mitigation]] — projective error mitigation
- [[Floquet Gate Calibration for Circuit Recompilation]] — often used alongside this technique
