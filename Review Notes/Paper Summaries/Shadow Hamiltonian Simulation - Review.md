# Review: Shadow Hamiltonian Simulation

Source note: [[Shadow Hamiltonian Simulation — Paper Notes]]

## Primary Sources Checked

- "Shadow Hamiltonian simulation," Nature Communications 16, 2690 (2025), DOI 10.1038/s41467-025-57451-z.

## Verdict

Major issues. The core invariance-property idea is captured, but the bibliographic metadata is incomplete and at least one complexity/advantage claim is too strong without theorem-level qualification.

## Line-Anchored Comments

- Lines 2-4: add the authors. Also fix the Nature Communications article number: DOI `s41467-025-57451-z` corresponds to article 2690, not 3486.
- Lines 17-21: the shadow-state definition should mention normalization edge cases. If all selected expectation values vanish, `A=0` and the normalized state is undefined.
- Lines 23-33: the invariance property is the central condition and is stated well. Add the sign/`i` convention carefully so the matrix `H_S` is genuinely the Hamiltonian generating the shadow evolution.
- Lines 44-50: "BQP-complete - genuine quantum advantage" needs exact scope. Free-fermion shadows can encode very large systems efficiently, but the note should say which input/output problem is BQP-complete rather than attaching BQP-completeness to the physical class as a whole.
- Lines 75-82: simulating `H_S` is only part of the algorithm. Efficient preparation of the initial shadow state and efficient extraction of desired observables must also be addressed.
- Lines 84-92: the comparison with classical shadows and shadow tomography is helpful and should stay.

## Missing or Suggested Additions

- Add a compact "efficiency checklist": closure under commutators, Hermiticity of `H_S`, sparse/evaluable `H_S`, initial shadow-state preparation, and readout of the desired expectation values.
