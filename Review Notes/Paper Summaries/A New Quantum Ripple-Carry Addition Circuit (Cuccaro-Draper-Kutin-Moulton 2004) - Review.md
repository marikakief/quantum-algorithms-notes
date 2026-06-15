# Review: A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004)

Source note: [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]]

## Primary Sources Checked

- Cuccaro, Draper, Kutin, and Moulton, "A new quantum ripple-carry addition circuit", arXiv: https://arxiv.org/abs/quant-ph/0410184
- ar5iv rendering of the paper: https://ar5iv.labs.arxiv.org/html/quant-ph/0410184

## Verdict

Minor issues. This is a strong summary. The resource counts and the MAJ/UMA story match the paper. A few statements should be scoped to the paper's counting model.

## Line-Anchored Comments

- Lines 9 and 15: qubit accounting

  Correct, but spell out the convention: the adder has two `n`-bit input registers, one carry-output wire, and one initialized ancilla. Saying "one ancilla" is right only after separating the output carry bit from scratch space.

- Lines 29-37: MAJ gate description

  Good. The source defines MAJ as two CNOTs followed by a Toffoli, storing the carry into one of the input wires.

- Lines 41-46: UMA variants

  Correct. The 3-CNOT UMA is needed for the depth-improved circuit; the 2-CNOT version is only the simpler conceptual version.

- Lines 54-76: depth optimizations and counts

  Good source match. The paper gives `2n-1` Toffoli gates, `5n-3` CNOTs, `2n-4` negations, and depth `2n+4` for `n >= 2`, with `2n-1` Toffoli time-slices plus 5 CNOT time-slices.

- Lines 84-96: variants

  Mostly good, but the table should explicitly say negations are usually not counted in size/depth in the paper's Table 1. The note lists NOT counts separately, which is fine, but readers may compare rows incorrectly.

- Line 114: T-count discussion

  Useful, but historically downstream. The Cuccaro paper itself is not a Clifford+T resource-estimation paper. Phrase this as "under later standard Clifford+T accounting" rather than as a claim internal to Cuccaro et al.

- Line 118: classical-to-quantum addition

  Correct and worth keeping. Cuccaro et al. explicitly note that a transform/QFT adder is superior for adding a compile-time classical constant because the classical addend need not be stored in quantum memory.

## Missing or Suggested Additions

- Add a one-line distinction between "clean ancilla", "output carry", and "borrowed/dirty ancilla"; this will prevent confusion with the HRS dirty-ancilla constant-adder note.
