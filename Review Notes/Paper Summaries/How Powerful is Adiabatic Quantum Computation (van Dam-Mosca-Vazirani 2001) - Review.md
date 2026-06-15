# Review: How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001)

Source note: [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]]

## Primary Sources Checked

- van Dam, Mosca, and Vazirani, "How powerful is adiabatic quantum computation?", FOCS 2001 / quant-ph/0206003.

## Verdict

Minor issues. The note is accurate and well scoped. It should be especially careful to distinguish the one-way simulation of adiabatic evolution by circuits from the later full equivalence theorem.

## Line-Anchored Comments

- Lines 7-15: the three-part summary is excellent. Keep the separation between search speedup, oracle informativeness, and a failure example.
- Lines 19-30: the circuit simulation section should be labelled as adiabatic-to-circuit simulation. It is not the later circuit-to-adiabatic universality construction of Aharonov et al.
- Lines 34-52: the local schedule derivation is good. Add that the schedule uses knowledge of the gap profile, which is available for unstructured search but not generally.
- Lines 56-68: the counting-oracle result is well explained. The caveat at lines 113-116 should be pulled earlier: reconstructing the objective does not make SAT easy.
- Lines 72-84: good summary of the perturbed Hamming-weight lower bound. Say "avoided crossing with exponentially small gap" rather than "level crossing" if discussing the actual interpolating Hamiltonian.
- Lines 111-116: the caveats are strong and should remain.

## Missing or Suggested Additions

- Add one sentence comparing this search result with Roland-Cerf: essentially the same local-adiabatic principle, independently developed, with Roland-Cerf giving the clean optimality presentation for search.

