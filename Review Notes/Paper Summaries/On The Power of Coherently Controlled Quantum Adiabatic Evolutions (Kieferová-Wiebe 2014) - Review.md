# Review: On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferova-Wiebe 2014)

Source note: [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]]

## Primary Sources Checked

- Kieferova and Wiebe, "On The Power Of Coherently Controlled Quantum Adiabatic Evolutions," New Journal of Physics 16, 123034 (2014) / arXiv:1403.6545.

## Verdict

Minor issues. The note captures the LCU/adiabatic-error-cancellation idea well. The cost discussion should be moved closer to the headline because the method uses an additional coherently controlled path.

## Line-Anchored Comments

- Lines 7-17: good framing. "Without requiring longer evolution time" should be paired immediately with the fact that one implements a coherent combination of two evolutions and pays success-probability/control overhead.
- Lines 23-31: the LCU gadget is well explained. "Repeat-until-success is practical" is true only in the adiabatic regime where the failure probability is already small; keep that qualifier.
- Lines 33-43: the first-order boundary-term explanation is excellent. It correctly links the result to Cheung-Hoyer-Wiebe.
- Lines 45-67: the path-design conditions are clear. Add that the second path can leave the intuitive monotone interpolation and may require Hamiltonian values outside the original schedule.
- Lines 69-75: the symmetric-spectrum global-cancellation case is important. Make clear this is a special structural condition, not generic.
- Lines 83-99: the query-complexity bound is too detailed without context. Say what model of sparse time-dependent simulation it assumes before displaying the formula.
- Lines 115-121: the caveats are good. The cost metric in the final caveat should be moved earlier.

## Missing or Suggested Additions

- Add a concise resource comparison: one ordinary adiabatic path versus two coherently controlled paths, success probability, Hamiltonian norm/time cost, and achieved diabatic-error order.

