
> **Source:** Aspuru-Guzik et al., Science 309 (2005); idea from Farhi, Goldstone, Gutmann, Sipser (2000)
> **Tags:** #trick #state-preparation #quantum-chemistry #adiabatic

## What it does
Prepares the ground state of a molecular Hamiltonian by slowly interpolating from a trivially preparable Hamiltonian to the target, relying on the adiabatic theorem to stay in the ground state.

## The trick

Define a path:

$$H(s) = (1-s) H_0 + s H_{\text{FCI}}, \quad s: 0 \to 1$$

where $H_0$ is chosen so its ground state is trivially preparable. A natural choice: $H_0 = H_{\text{HF}}$ is diagonal in the Hartree-Fock basis, with only the HF energy as its non-trivial element, so $|\text{HF}\rangle$ is the ground state.

Discretise $s$ into $T$ steps and simulate the time-dependent evolution:

$$|\psi(s_{k+1})\rangle \approx e^{-iH(s_k)\Delta s} |\psi(s_k)\rangle$$

The adiabatic theorem guarantees the system stays in the ground state if $\Delta s \ll (\Delta E)^2$, where $\Delta E$ is the minimum spectral gap along the path. The resulting state has high overlap with the true ground state, making [[Gapped Phase Estimation|phase estimation]] succeed with good probability.

## When to reach for it
- When Hartree-Fock has poor overlap with the true ground state (stretched bonds, strongly correlated systems)
- As a front-end to phase estimation in any quantum chemistry pipeline
- When you have a good classical estimate of the gap along the interpolation path

## Complexity
- $T$ Trotter steps, each requiring one [[Hamiltonian simulation]] step
- $T$ must be large enough that $1/T \ll (\min_s \Delta E(s))^2$
- For well-behaved gaps: $T = O(\mathrm{poly}(1/\Delta E_{\min}))$

## Caveat
The gap $\Delta E(s)$ can close (or become exponentially small) for strongly correlated systems, phase transitions, or conical intersections. When the gap closes, adiabatic state preparation fails — you need exponentially many steps. Predicting the gap along the path is itself a hard problem in general, though classical methods (CASSCF, DMRG) can give lower bounds for many practical cases.

Also, each Trotter step incurs its own simulation error, so the total error is the product of adiabatic error and Trotter error.

## Related notes
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]]
- [[Gapped Phase Estimation]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
