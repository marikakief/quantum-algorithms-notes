# Review: Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024)

Source note: [[Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) — Paper Notes]]

## Primary Sources Checked

- Oumarou et al., "Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization", arXiv:2212.07957 / Quantum 8, 1371 (2024).
- Checked against later SCDF context at arXiv:2403.03502 to separate RC-DF from symmetry-compressed variants.

## Verdict

Major issues. The RC-DF explanation is useful, but the resource-number discussion appears to blur RC-DF with later symmetry-compressed double-factorization work.

## Line-Anchored Comments

- Lines 19-26: the regularization motivation is good. It should say that the regularizer is a heuristic tied to preserving correlated-energy quality while reducing quantum cost; it is not a theorem that CCSD(T)-motivated regularization optimizes QPE cost.
- Line 26: the `2.6e9` FeMoCo number is suspicious in this context. The arXiv abstract for RC-DF emphasizes CpdI/P450 with 58 orbitals and roughly halving qubitized runtime; very low FeMoCo Toffoli numbers are more naturally associated with later symmetry-compressed/shifted approaches. Verify the table before attributing `2.6e9` to RC-DF alone.
- Lines 68-72: if the `lambda ~700`, `2.6e9` Toffoli, and `1994` logical-qubit values are imported from another paper, the note needs to name that paper and not present them as the direct headline output of Oumarou et al. 2024.
- Line 87: the comparison table should separate RC-DF, SCDF, BLISS-THC, and spectrum-amplified DFTHC. They are related compression strategies, but they are not interchangeable baselines.
- Lines 108-110: the duplicate/floating links should be cleaned up after the resource table is verified.

## Missing or Suggested Additions

- Add an explicit "RC-DF versus SCDF versus BLISS" paragraph. This cluster has enough similarly named compressed-factorization methods that readers need a map.

