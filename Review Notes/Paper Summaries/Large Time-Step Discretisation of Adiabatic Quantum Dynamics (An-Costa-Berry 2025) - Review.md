# Review: Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025)

Source note: [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]

## Primary Sources Checked

- An, Costa, and Berry, "Large time-step discretisation of adiabatic quantum dynamics", arXiv:2509.00171.

## Verdict

Minor issues. The note is strong and captures the main conceptual shift to discrete adiabatic walk operators. The main correction is to keep "super-polynomial" and "exponential" convergence language distinct.

## Line-Anchored Comments

- Lines 17-23: good headline. The caveat about Conjecture 10 is essential and is placed early enough.
- Line 11: typo: "[[Product Formulas]]s" has an extra `s`. This appears in several later lines too.
- Lines 31-37: excellent explanation of why the paper avoids the usual continuous-evolution-plus-discretisation-error split.
- Lines 49-59: the gap-matching discussion is clear. It is important that `h` is independent of `epsilon` and `T`, not necessarily universally large.
- Lines 65-71: good conditional statement. If only a finite number of derivatives vanish, the order should be finite; all-orders boundary cancellation is what supports the "for any `k`" super-polynomial discussion.
- Lines 73-83: the "Trotter opens gaps" section is interesting but speculative. The note correctly says this is demonstrated on toy models; keep that caveat near any algorithmic-consequence claim.
- Lines 107-118: theorem bullets correctly mark the boundary-cancellation/Grover results as conditional where needed.
- Line 136: "achieves exponential convergence" should be changed to "super-polynomial convergence" or "faster than any inverse polynomial under the conjecture." The note elsewhere states `O(1/epsilon^{1/k})` for arbitrary `k`, which is not the same as a proved exponential rate.
- Line 180: Kieferova-Scherer-Berry is usually cited as 2018 for the arXiv/paper title in this vault; keep the year convention consistent.

## Missing or Suggested Additions

- Add a short warning that the walk-operator spectral gap is an additional object to analyze; it is not automatically the Hamiltonian gap at large step size.
