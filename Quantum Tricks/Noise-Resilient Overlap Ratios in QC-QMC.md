
> **Source:** Huggins, O'Gorman, Rubin, Reichman, Babbush, Lee, arXiv:2106.16235
> **Tags:** #trick #noise-resilience #QMC #AFQMC #NISQ #error-mitigation

## What it does

Makes the phaseless AFQMC constraint naturally immune to depolarizing noise on the quantum device, without any error mitigation protocol. The key quantity — the overlap *ratio* between consecutive walker states and the trial — is invariant under noise channels that rescale all overlaps uniformly.

## The trick

AFQMC needs the overlap ratio:

$$S_i(\tau) = \frac{\langle \Psi_T | \phi_i(\tau + \Delta\tau) \rangle}{\langle \Psi_T | \phi_i(\tau) \rangle}$$

Under global depolarizing noise $\rho \mapsto (1-p)\rho + p\,I/2^N$, each overlap $\langle \phi | \Psi_T \rangle = 2\langle \phi | \rho | 0 \rangle$ picks up a factor $(1-p)$:

$$2\langle \phi | \rho' | 0 \rangle = 2\left[(1-p)\langle \phi | \rho | 0 \rangle + \frac{p}{2^N}\langle \phi | 0 \rangle\right] = (1-p) \cdot 2\langle \phi | \rho | 0 \rangle$$

The second term vanishes because $\langle \phi | 0 \rangle = 0$ for any state with nonzero particle number (the walker and vacuum live in different particle-number sectors). The $(1-p)$ factor cancels in the ratio.

This extends to:
- **Robust shadow tomography** (Chen et al. 2021): The noise parameter $f^{-1}$ in the inverse channel $\mathcal{M}^{-1}$ drops out of the ratio. No calibration needed.
- **Partitioned shadow tomography** (spin-sector split): The cancellation still holds as long as each partition has nonzero particle number.
- Any noise channel that acts as a multiplicative rescaling on all overlaps simultaneously.

## When to reach for it

- Whenever a quantum algorithm's output depends on *ratios* of quantities rather than their absolute values.
- Designing hybrid algorithms for NISQ devices where error mitigation is expensive.
- The principle is broader than QMC: any algorithm that uses overlap ratios, probability ratios, or amplitude ratios can potentially benefit.

## Complexity

Zero overhead. The noise cancellation is structural — it follows from particle-number conservation and the form of the estimator. No additional circuits, measurements, or classical processing.

## Caveat

Only works for noise channels that rescale all overlaps by the same factor. State-preparation errors that distort the trial wavefunction's phase structure are *not* cancelled — they change the quality of the constraint, not just its scale. Coherent errors, non-Markovian noise, and gate-dependent noise may break the cancellation.

Also, depolarizing noise increases the *variance* of the overlap estimates even though it doesn't bias the ratio. At high noise rates, you need more shadow tomography samples to achieve the same statistical precision.

## Related notes

- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]]
- [[Shadow Tomography for QMC Overlap Estimation]] — the measurement protocol where this resilience applies
- [[Variational Quantum Eigensolver (VQE)]] — VQE has some noise resilience via the variational principle, but it's a weaker form (upper bound still holds, but noise adds a positive bias to the energy)
- [[Pauli Expectation Value Estimation]] — expectation values don't enjoy this ratio-based cancellation
