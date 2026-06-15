# Review: Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000)

Source note: [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]

## Primary Sources Checked

- Farhi, Goldstone, Gutmann, and Sipser, "Quantum Computation by Adiabatic Evolution," arXiv:quant-ph/0001106 (2000).

## Verdict

Minor issues. The note is clear and useful. It should distinguish the original linear-schedule proposal from later local-adiabatic schedules and from the later formal equivalence theorem.

## Line-Anchored Comments

- Lines 7-11: good opening. Add that this paper proposes the model and studies examples; polynomial equivalence to circuits is a later theorem.
- Lines 27-35: the beginning-Hamiltonian section is mostly right, but the notation `|x_i = 0>` is potentially confusing. These are `X`-basis ground states, whose computational-basis expression is the uniform superposition.
- Lines 43-45: the adiabatic runtime statement is fine as heuristic/theorem-level guidance. Add that matrix elements and schedule smoothness matter, not only the minimum gap.
- Lines 51-57: the avoided-crossing discussion is useful but should not imply large gaps. Generic avoided crossings can still have exponentially small gaps.
- Lines 59-66: the example table is good. For Grover, specify that the no-speedup conclusion is for the simple/global linear schedule analyzed here; Roland-Cerf local adiabatic scheduling recovers the quadratic Grover speedup.
- Lines 88-102: the Grover derivation is well summarized. The last sentence should say "no speedup for this schedule" rather than "no speedup" absolutely.
- Lines 106-114: this is a simulation-by-circuits observation, not the later Aharonov et al. equivalence proof in the hard direction. The note should keep those distinct.
- Lines 118-122: the bush-of-implications lesson is a good addition. Keep the warning that low-energy density alone does not determine the gap.

## Missing or Suggested Additions

- Add a short comparison with local adiabatic search: same problem Hamiltonian family, different schedule, different query complexity.

