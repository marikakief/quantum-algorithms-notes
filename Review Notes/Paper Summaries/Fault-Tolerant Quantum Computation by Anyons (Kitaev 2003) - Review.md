# Review: Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003)

Source note: [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]

## Verdict

Major issues. The toric-code and quantum-double exposition is detailed, but the introductory claims overstate passive protection and universality. The note should distinguish the abelian toric-code memory, finite-group quantum-double models, and later universal anyon models much more sharply.

## Line-Anchored Comments

- Lines 7-17: "Thermal relaxation automatically removes excitations" is too strong. The 2D toric code is not a self-correcting quantum memory at finite temperature; anyon creation and diffusion can create logical errors. State protection as an energy gap plus topological error-detection/correction structure, not automatic thermal healing.

- Lines 15-17: "Non-abelian anyons from quantum doubles ... giving universal quantum computation via braiding" is overbroad. Finite-group quantum doubles have rich non-abelian statistics, but braiding alone is not automatically universal. Universality is model-dependent and may require measurements, ancillas, or different modular categories.

- Lines 35-46: The toric-code parameters and loop logicals are good. Add the distinction between distance `k` and random-error threshold/percolation behavior; "no limit on number of correctable errors" should mean below-threshold random errors, not arbitrary high-weight errors.

- Lines 57-61: The abelian anyon description is good. "Neither bosonic nor fermionic" should be phrased as mutual semionic statistics between electric and magnetic charges; individual `e` and `m` excitations in the toric code are bosonic, while their composite is fermionic.

- Lines 77-98: The protected-space explanation is valuable, but it needs a clearer separation between local charge degrees of freedom and fusion/protected degrees. Also avoid implying that `S_3` universality is settled here.

- Lines 104-115: The key-results table should mark "braiding universality depends on G" as an open/model-dependent issue, not a contribution equivalent to later SU(2)_k density theorems.

## Missing or Suggested Additions

- Add a short comparison: toric code memory vs non-abelian quantum double computation vs Fibonacci/SU(2)_k universal braiding.
- Add a finite-temperature caveat near the first paragraph.
