# Review: Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999)

Source note: [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]]

## Verdict

Minor issues. The note explains gate teleportation and the Clifford hierarchy well. The main fixes are to make qubit ordering/corrections precise and avoid over-crediting the paper with later magic-state-distillation machinery.

## Line-Anchored Comments

- Lines 7-13: The summary is good. Add that universality still requires a suitable universal set of resource states or gates; teleportation is the implementation primitive, not a free source of non-Clifford resources.

- Lines 17-29: The CNOT resource-state formula should state the qubit ordering. The output line `CNOT |beta>|alpha>` may confuse readers about which input is control and which is target.

- Lines 35-46: The Clifford hierarchy definition is correct in spirit. Mention that global phases are usually quotiented out and that `C_k` is not a group for `k >= 3`.

- Lines 48-60: The byproduct-correction explanation is strong. Add that adaptivity/recursive correction is what makes third-level gates tractable in fault-tolerant settings.

- Lines 62-74: The resource-state preparation paragraph risks sounding like a practical magic-state factory. The paper gives a systematic fault-tolerant preparation idea; distillation thresholds and modern magic-state factories came later.

- Lines 76-86: "Universal QC from Bell measurements + GHZ states" should keep "plus single-qubit operations/resource preparation" in the same sentence.

- Lines 99-105: The caveats are good. Add Eastin-Knill/context: teleportation through resource states is one way around limitations of transversal gate sets, but the non-Clifford resource has to be paid for elsewhere.

## Missing or Suggested Additions

- Add the standard resource-state identity `(I \otimes U)|Phi+>` with the Pauli byproduct update table for `T` or Toffoli as an example.
- Add a short comparison with later Bravyi-Kitaev magic-state distillation and Raussendorf-Briegel MBQC.
