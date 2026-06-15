
> **Source:** Aspuru-Guzik et al., Science 309 (2005); idea from Farhi, Goldstone, Gutmann, Sipser (2000)
> **Tags:** #trick #state-preparation #quantum-chemistry #adiabatic

## What it does
Prepares the ground state of a molecular Hamiltonian by slowly interpolating from a trivially preparable Hamiltonian to the target, relying on the adiabatic theorem to stay in the ground state.

## The trick

Define a path:

$$H(s) = (1-s) H_0 + s H_{\text{FCI}}, \quad s: 0 \to 1$$

where $H_0$ is chosen so its ground state is trivially preparable. A natural choice is an $H_{\text{HF}}$-like initial Hamiltonian whose ground state is the Hartree-Fock determinant in the chosen encoding/order.

Choose a physical-time schedule $s(t)$ from 0 to 1 and simulate the time-dependent evolution over intervals $\Delta t$:

$$|\psi(t_{k+1})\rangle \approx e^{-iH(s(t_k))\Delta t} |\psi(t_k)\rangle$$

More accurately, a discretized implementation approximates factors like $e^{-iH(s(t_k))\Delta t}$. The adiabatic theorem bounds error in terms of the total runtime, path-derivative norms such as $\|dH/ds\|$, the schedule, and inverse powers of the minimum spectral gap. A common heuristic is that runtime must scale at least like $\|dH/ds\|/\Delta_{\min}^2$. The resulting state has high overlap with the true ground state only when the chosen path remains sufficiently gapped.

## When to reach for it
- When there is evidence for a gapped path from an easy reference such as Hartree-Fock to the target state. Poor Hartree-Fock overlap alone is not enough if the interpolation gap closes.
- As a front-end to phase estimation in any quantum chemistry pipeline
- When you have a good classical estimate of the gap along the interpolation path

## Complexity
- Total physical runtime and the number of simulation segments are separate choices.
- Runtime depends on the schedule, $\|dH/ds\|$ and higher derivatives, and inverse powers of $\Delta_{\min}$.
- The number of Trotter/simulation steps then depends on the Hamiltonian-simulation error budget for that time-dependent path.

## Caveat
The gap $\Delta E(s)$ can close (or become exponentially small) for strongly correlated systems, phase transitions, or conical intersections. When the gap closes, adiabatic state preparation fails — you need exponentially many steps. Predicting the gap along the path is itself a hard problem in general, though classical methods (CASSCF, DMRG) can give lower bounds for many practical cases.

The total error budget has at least three pieces: adiabatic error, Hamiltonian-simulation/Trotter error for the time-dependent path, and final phase-estimation projection/readout error.

## Related notes
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]]
- [[Gapped Phase Estimation]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
