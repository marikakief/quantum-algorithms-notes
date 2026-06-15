# Review: Halving the Cost of Quantum Addition (Gidney 2018)

Source note: [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]]

## Primary Sources Checked

- Gidney, "Halving the cost of quantum addition", arXiv: https://arxiv.org/abs/1709.06648
- ar5iv rendering of the paper: https://ar5iv.labs.arxiv.org/html/1709.06648
- Quantum journal page: https://quantum-journal.org/papers/q-2018-06-18-74/

## Verdict

Minor issues. The note is technically strong. The main caveat is that the opportunity-cost crossover depends on memory assumptions, and the temporary-AND replacement condition should remain prominent.

## Line-Anchored Comments

- Lines 13-15: headline result

  Correct. The source abstract gives the improvement from `8n + O(1)` to `4n + O(1)` T gates and identifies the temporary logical-AND as the conceptual contribution.

- Lines 21-45: compute/erase asymmetry

  Good. The source says the logical-AND computation has T-count 4 and the uncomputation uses measurement and Clifford fixup with T-count 0. Keep the statement that the consumed `|T>` state counts as part of the T resource.

- Lines 51-54: replacing paired Toffolis

  Correct for the Cuccaro-style adder structure. The condition is not merely "there is a later uncompute"; intermediate operations must be insensitive to the temporary phase/entanglement condition shown in Gidney's Fig. 5.

- Lines 81-85: opportunity cost

  Valuable section, but line 83 is too crisp. The source gives a rough cutoff near 960 under one storage model and notes a memory-compression assumption can move it to about 5760. For `n=4096`, the conclusion "well within this range" is true only under the memory-compressed idle-qubit assumption, not under the base 960 estimate.

- Line 104: Shor estimate reduction

  Good source match: Gidney quotes reduction of a previous surface-code factoring estimate from about 27 hours to 9 hours in measurement-depth terms and from about `2e12` to `8e11` distilled states under the stated assumptions.

- Line 115: "any circuit"

  Too broad. Temporary logical-ANDs apply to compute/uncompute Toffoli pairs satisfying the phase-insensitivity condition. Grover oracles built as clean compute-phase-uncompute reversible predicates are a good example, but arbitrary oracle implementations may not expose the needed paired structure cheaply.

## Missing or Suggested Additions

- Add a separate "raw T-count vs effective T-count" paragraph, because the paper's most subtle point is that an ancilla-heavy circuit can lose its apparent T-count advantage after surface-code layout costs are included.
