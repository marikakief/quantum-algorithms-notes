# Review: Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala-Mezzacapo-Temme-Takita-Brink-Chow-Gambetta 2017)

Source note: [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes]]

## Primary Sources Checked

- Kandala et al., "Hardware-efficient variational quantum eigensolver for small molecules and quantum magnets," Nature 549, 242-246 (2017) / arXiv:1704.05018.

## Verdict

Major issue in the optimizer-cost sentence; otherwise minor. The note captures the hardware-efficient ansatz paradigm well, but it misstates SPSA's per-iteration query scaling.

## Line-Anchored Comments

- Lines 7-11: the problem statement is good. "Outperform physically motivated ansatze" should be tied to circuit depth and this hardware, not interpreted as a general accuracy claim.
- Lines 15-25: good description of the HE-VQE paradigm. Mention that CNOT is implemented through calibrated cross-resonance-style native interactions; CNOT is not a primitive in the same abstract sense as a one-qubit rotation.
- Lines 31-41: the qubit reductions are useful. Add that frozen-core/active-space choices and symmetry tapering change the physical problem being solved.
- Lines 43-50: the ansatz description is fine. The lack of particle-number/spin conservation should be mentioned here, not only in limitations, because it affects interpretation of chemistry results.
- Lines 52-61: correction needed. SPSA estimates a stochastic gradient using two objective evaluations per iteration, independent of the number of parameters, not `O(params)` evaluations as in coordinate finite differences. Total shots still can be large because each objective evaluation estimates many Pauli terms.
- Lines 74-82: the table and text conflict slightly. If LiH is listed at order `10^-2` Hartree, it is not approximately chemical accuracy; chemical accuracy is about `1.6e-3` Hartree.
- Lines 84-89: the gate-count comparison with UCC is fair, but "lower energies than UCCSD at the same circuit depth" should be scoped to the studied encodings, depths, optimizer behavior, and hardware noise.
- Lines 105-112: the limitations are excellent. The barren-plateau caveat belongs near the ansatz description as well.

## Missing or Suggested Additions

- Add a sentence distinguishing ansatz expressibility from symmetry correctness. A hardware-efficient circuit may be expressive, but if it explores unphysical particle-number sectors the low energy can be harder to interpret without penalties or postselection.

