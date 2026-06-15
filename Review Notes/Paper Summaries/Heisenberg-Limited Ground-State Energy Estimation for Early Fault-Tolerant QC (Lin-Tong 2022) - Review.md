# Review: Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022)

Source note: [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]

## Primary Sources Checked

- Lin and Tong, "Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant Quantum Computers," PRX Quantum 3, 010318 (2022), arXiv:2102.11340.

## Verdict

Minor issues. This is a clear and useful summary of the ACDF method, and the resource measures are the right ones. The main corrections are about careful comparison with QPE, avoiding overbroad claims about extracting arbitrary spectral data, and cleaning up one bibliographic/reference mismatch.

## Line-Anchored Comments

- Lines 7-11: the opening is accurate, but "short maximal evolution time" should include the polylogarithmic overhead that appears later in line 80. Otherwise it sounds exactly `O(epsilon^-1)` rather than `tilde O(epsilon^-1)`.
- Lines 15 and 22: good use of `eta` as a probability lower bound. Cross-links to notes using `gamma` should explicitly say `eta = gamma^2` if `gamma` denotes amplitude overlap.
- Lines 26-31: the QPE comparison is directionally right, but "or worse" is imprecise. Textbook QPE, semiclassical QPE, and high-confidence variants have different tradeoffs in ancilla count, repetitions, and maximum coherent time; the table later is a better place for this comparison.
- Lines 39-43: the Hadamard-test description should mention that controlled Hamiltonian evolution is the quantum primitive. This matters because the algorithm is early fault-tolerant, not a control-free analog-simulation method.
- Lines 55-59: the importance-sampling estimator is well explained. If space permits, add that the real CDF estimate is obtained from the appropriate real part/statistic, since `Z` is written as a complex random variable.
- Lines 61-69: the CERTIFY description is good. Emphasize that the output is a promised fuzzy decision with a transition width, not an exact comparison to the discontinuous CDF.
- Lines 96-104: "spectral Swiss army knife" overstates the reusable ACDF. Excited-state energies, gaps, and density-of-states features are accessible only to the extent that the spectral measure has enough weight and the target features are resolvable at the chosen smoothing width.
- Lines 106-110: the QET-U connection is useful, but the named follow-up should be checked. The QET-U ground-state/energy-estimation note in this vault is Dong-Lin-Tong 2022, not "Dong, Lin, Ni, and Wang" if this is meant to refer to arXiv:2204.05955.
- Lines 112-118: the caveats are excellent. The Hamiltonian-simulation overhead bullet should be moved closer to the construction because it is part of the access model, not just a practical afterthought.
- Line 132: "improved in this paper's Appendix C to `epsilon^-4`" reads like a typo because the preceding clause already says `epsilon^-4`. Clarify what was improved, or remove the phrase.

## Missing or Suggested Additions

- Add one sentence that this algorithm estimates the ground energy but does not project or prepare the ground state. The note says this in the caveats; it deserves to be near the top because it distinguishes the paper from Lin-Tong 2020 and QET-U preparation algorithms.

