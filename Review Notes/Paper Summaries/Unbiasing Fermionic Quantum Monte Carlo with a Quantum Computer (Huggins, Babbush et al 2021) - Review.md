# Review: Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins-Babbush et al. 2021)

Source note: [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]]

## Primary Sources Checked

- Huggins et al., "Unbiasing fermionic quantum Monte Carlo with a quantum computer," Nature 603, 416-420 (2022) / arXiv:2106.16235.

## Verdict

Minor-to-major issues. The conceptual summary is strong and appropriately distinguishes QC-QMC from VQE. The main technical concern is that the classical-shadow measurement scaling is written too cleanly; the observable-dependent shadow norm and overlap-decay issues need to be visible wherever the logarithmic-in-queries claim appears.

## Line-Anchored Comments

- Lines 15-21: excellent high-level framing. Keep the point that the quantum device supplies a trial constraint rather than directly estimating the energy.
- Lines 39-50: the overlap/local-energy reduction is good. Add that local energy evaluation needs many overlap ratios and that stability depends on denominators not becoming too small.
- Lines 52-64: the shadow-tomography paragraph is too simplified. Classical shadows scale logarithmically in the number of target observables after fixing the relevant shadow norm; for nonlocal overlap operators, that norm and the overlap magnitude can dominate the cost.
- Lines 66-74: good honesty about no demonstrated quantum advantage from the PP trial. This caveat should be moved nearer to the experimental claims, not only left in the algorithm section.
- Lines 76-84: the active-space/virtual-correlation explanation is valuable. "For free" should be softened to "without additional quantum resources"; there is still classical contraction and AFQMC cost.
- Lines 86-92: the depolarizing-noise cancellation argument is useful, but exact cancellation holds under specific noise/orthogonality assumptions. For more general noise, robust-shadow calibration or stationarity assumptions are needed.
- Lines 96-116: the experimental summaries are clear. Make explicit that the quantum active spaces are small even when the AFQMC orbital spaces are large.
- Lines 118-120: repeat the warning about shadow norms here. `O(log M / epsilon^2)` by itself is not a complete scalability statement.
- Lines 139-149: the limitations section is very good and should remain. It already contains the overlap-decay caveat that needs to be cross-linked earlier.

## Missing or Suggested Additions

- Add a small "what would constitute advantage?" paragraph: a trial family with classically hard overlap evaluation, controlled overlap decay, efficient shadow or matchgate-shadow estimation, and improved AFQMC bias on chemically relevant systems.

