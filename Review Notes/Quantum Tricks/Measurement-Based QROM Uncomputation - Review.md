# Review: Measurement-Based QROM Uncomputation

Source note: [[Measurement-Based QROM Uncomputation]]

## Primary Sources Checked

- Berry et al., arXiv:1902.02134, Appendix C.
- Gidney-style measurement-based uncomputation background from prior arithmetic primitives.

## Verdict

Major issues. The idea is right, but the card states the post-measurement output register becomes `|0>`, which is not what an X-basis measurement does.

## Line-Anchored Comments

- Lines 9-16: the phase-fixup intuition is good: X-basis measurement of a deterministic data register induces address-dependent phases that can be corrected classically.
- Line 11: "does not disturb the superposition over `j`" needs a qualifier. The address amplitudes remain coherent after the proper phase correction; before correction, the measurement outcome has imposed a known phase pattern.
- Line 18: incorrect. X-basis measurement collapses the output qubits to X-basis classical outcomes, not to `|0>`. The register is consumed by measurement and can then be reset/reused. Do not describe this as coherent erasure to `|0>`.
- Lines 20-22: the cost formulas are useful. They should be introduced as the cost of the phase-fixup QROM, not as "the output-register uncompute" in the usual inverse-circuit sense.
- Line 24: "classically conditioned Z corrections are free in the surface code" is too compressed. Pauli-frame updates are cheap, but the address-dependent phase pattern may require a nontrivial fixup lookup, which the preceding lines count.
- Line 38: the real-time classical computation caveat is good. Keep it; this is often ignored in resource estimates.

## Missing or Suggested Additions

- Replace "output register is now `|0>`" with "the output register has been measured and can be reset after the phase fixup."
- State the precondition: the data register must no longer be needed coherently except for erasure.

