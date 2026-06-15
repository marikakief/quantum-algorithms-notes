# Review: Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011)

Source note: [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]

## Primary Sources Checked

- Temme, Osborne, Vollbrecht, Poulin, and Verstraete, "Quantum Metropolis Sampling," Nature 471, 87-90 (2011), arXiv:0911.3635.

## Verdict

Minor-to-major issues. The algorithmic narrative is good, but the note overstates the sign-problem and general-purpose claims, and it incorrectly describes the Marriott-Watrous rejection recovery as exponentially convergent despite displaying an inverse-linear bound.

## Line-Anchored Comments

- Lines 17-23: per-step implementability is not the same as an efficient sampler. The total runtime also depends on the quantum Markov-chain mixing time and on phase-estimation precision.
- Line 19: "evading the fermion sign problem" is acceptable as a motivation, but too strong as a blanket algorithmic claim. The method avoids classical negative-weight Monte Carlo sampling by sampling the quantum eigenbasis, yet Hamiltonian simulation, phase estimation, and mixing can still be prohibitive.
- Line 22: "recovers the original state with exponentially high probability" conflicts with the bound shown later at line 58.
- Lines 31-48: the step description is clear. Add that the "known energy" is stored/maintained through phase-estimation registers up to finite precision; degeneracies and approximate energies are part of the real implementation.
- Lines 57-60: the displayed bound `p_fail(n) <= 1/(2e(n+1))` is inverse-linear, not exponential. Therefore line 57, line 78, and line 111 should not say "exponential convergence" unless a different amplified procedure is being cited.
- Lines 66-69: detailed balance plus ergodicity gives the Gibbs state as fixed point/unique fixed point. A universal proposal set helps ergodicity, but does not by itself imply rapid mixing.
- Lines 77-82: the key-results table should distinguish exact phase estimation from finite precision and should keep the ergodic coefficient/error denominator visible.
- Lines 88-95: the significance paragraph is rhetorically strong but too broad. Quantum Metropolis is a principled route to equilibrium sampling, not a general-purpose black-box solver for Hubbard, chemistry, or lattice gauge theory without mixing-time qualifications.
- Lines 101-105: the caveats are good and should be moved earlier or referenced from the headline claims.

## Missing or Suggested Additions

- Add a "what is proved versus hoped" sentence: the paper proves the correct fixed point and a valid quantum accept/reject mechanism; it does not prove rapid mixing for generic many-body Hamiltonians.

