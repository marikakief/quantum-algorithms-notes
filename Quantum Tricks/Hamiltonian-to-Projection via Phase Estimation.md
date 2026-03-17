# Hamiltonian-to-Projection via Phase Estimation

> **Source:** Aharonov & Ta-Shma, arXiv:quant-ph/0301023 (2003), Claim 6
> **Tags:** #trick #phase-estimation #projection #adiabatic

## What it does

Converts a simulatable Hamiltonian $H$ (with known spectral gap) into a simulatable projector $\Pi_H = I - |\alpha\rangle\langle\alpha|$, where $|\alpha\rangle$ is the ground state of $H$. This lets you work with projectors — which have gap exactly 1 — instead of arbitrary Hamiltonians.

## The trick

Given a simulatable $H$ with spectral gap $\Delta(H) \geq 1/n^c$ and ground energy 0:

1. Use Kitaev's [[Gapped Phase Estimation|phase estimation]] on $e^{-iHt}$ to estimate the eigenvalue of an input eigenstate. Since $\Delta(H) \geq 1/n^c$, choosing precision $O(1/n^c)$ distinguishes the ground state from all excited states with exponentially good confidence.

2. Write a flag bit: $|0\rangle$ if ground state, $|1\rangle$ if not.

3. Apply conditional phase $e^{-it}$ on the flag bit (conditioned on $|1\rangle$). This implements evolution under a Hamiltonian that is 0 on the ground state and 1 on its orthogonal complement — exactly $\Pi_H$.

4. Uncompute the flag bit by reversing the [[Gapped Phase Estimation|phase estimation]].

## When to reach for it

- [[Jagged Adiabatic Path via Projections|Jagged adiabatic path construction]]: need projectors with gap 1 as stepping stones
- Any adiabatic algorithm where you want to replace a Hamiltonian with a cleaner spectral structure
- Ground state preparation via projection + [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]]

## Complexity

- Requires $O(1/\Delta(H))$ uses of $e^{-iHt}$ for the [[Gapped Phase Estimation|phase estimation]] step
- Plus the cost of simulating $H$ for those times
- Total overhead per use of $\Pi_H$: $\mathrm{poly}(n, 1/\Delta)$

## Caveat

Only works when the spectral gap is known and inverse-polynomially large. If $\Delta$ is exponentially small, the phase estimation step becomes exponentially expensive.

## Related notes

- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]]
- [[Jagged Adiabatic Path via Projections]]
- [[Gapped Phase Estimation]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]
