# Parent Hamiltonian Witness for State Certification

> **Source:** Cramer, Plenio, Flammia, Somma, Gross, Bartlett, Landon-Cardinal, Poulin, Liu, arXiv:1101.4366
> **Tags:** #trick #MPS #certification #parent-Hamiltonian #fidelity-bound #tomography

## What it does
Certifies the fidelity of a reconstructed MPS estimate by constructing its parent Hamiltonian and using it as an entanglement witness. The fidelity bound requires no assumptions about the true state.

## The trick

A generic (injective) MPS $|\psi\rangle$ with bond dimension $D$ is the unique ground state of a local parent Hamiltonian $\hat{H} = \sum_i \hat{h}_i$ where each $\hat{h}_i$ is a projector onto the subspace orthogonal to the range of the MPS transfer map on $k$ sites. This Hamiltonian has $|\psi\rangle$ as ground state with energy 0 and a spectral gap $\Delta E > 0$.

The fidelity bound:

$$\langle\psi|\hat{\varrho}|\psi\rangle \geq 1 - \frac{\sum_i (\varepsilon_i + \text{tr}[\hat{h}_i \hat{\sigma}_i])}{\Delta E}$$

where $\hat{\sigma}_i$ are the measured local reduced density matrices and $\varepsilon_i = \|\hat{\varrho}_i - \hat{\sigma}_i\|_{\text{tr}}$ is the measurement precision.

**Why this is tight:** If the true state is $|\psi\rangle$ and measurements are exact, then $\text{tr}[\hat{h}_i \hat{\sigma}_i] = 0$ for all $i$ (ground state has zero energy) and $\varepsilon_i = 0$, giving fidelity = 1.

**Computing the gap:** The parent Hamiltonian projectors and gap are computable from the MPS tensors in polynomial time via the inequality:

$$\hat{H}^2 \geq (1-\gamma)\hat{H}, \qquad \gamma = \max_n \sum_{m: 1 \leq |n-m|k \leq 2k} \gamma_{n,m}$$

where $\gamma_{n,m}$ is determined by the smallest non-zero eigenvalue of $\hat{P}_n + \hat{P}_m$ (pairs of neighbouring projectors). So $\Delta E \geq 1 - \gamma$.

## When to reach for it

- Certifying the output of any MPS preparation algorithm on quantum hardware
- Providing a rigorous fidelity guarantee for [[Sequential Disentanglement for MPS Tomography|sequential disentanglement tomography]] or [[MPS-SVT for Polynomial State Reconstruction|MPS-SVT reconstruction]]
- Post-hoc verification of ground state preparation for frustration-free Hamiltonians
- Benchmarking quantum simulators: measure local observables, compare against the parent Hamiltonian expectations

## Complexity

- Constructing the parent Hamiltonian from MPS tensors: $O(N \cdot D^6 d^k)$ (dominated by range computation for each projector)
- Computing the gap lower bound: $O(N \cdot D^{12} d^{2k})$ (eigenvalue problems for pairs of projectors)
- Both polynomial in $N$ for fixed $D$ and $d$

## Caveat

- **Requires injectivity.** Non-injective MPS (e.g., GHZ states) have degenerate parent Hamiltonians. The method still certifies overlap with the ground space, but additional measurements (e.g., string operators) are needed to pin down the state within that space.
- **Gap can be small.** Near quantum phase transitions, $\Delta E \to 0$ and the bound becomes vacuous. The gap is a property of the reconstructed state, not the true state, so this is detectable.
- **Only certifies overlap with a single MPS.** If the true state is a mixture of MPS or has bond dimension larger than assumed, the parent Hamiltonian of the reconstructed MPS won't catch that — though the measured energy $\sum_i \text{tr}[\hat{h}_i \hat{\sigma}_i]$ will be nonzero, flagging the issue.

## Related notes
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]]
- [[Sequential Disentanglement for MPS Tomography]]
- [[MPS-SVT for Polynomial State Reconstruction]]
- [[MPS Canonical Form for Efficient State Tracking]]
