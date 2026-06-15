# Review: Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019)

Source note: [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]]

## Primary Sources Checked

- Childs, Ostrander, and Su, "Faster quantum simulation by randomization", Quantum 2019 / arXiv:1805.08385.

## Verdict

Minor issues. The note is clear and captures the cancellation mechanism. A few claims should be limited to proved bounds rather than actual performance for every fixed ordering.

## Line-Anchored Comments

- Lines 9-10: "provably stronger error bounds than any fixed ordering" is too strong. The paper proves improved randomized-channel bounds; it does not prove that every deterministic ordering performs worse on every Hamiltonian.
- Lines 15-25: good description of the random-permutation and first-order variants.
- Lines 31-41: excellent explanation of nondegenerate-term cancellation.
- Lines 47-57: theorem summaries are useful. It would help to define `Lambda` locally in this section.
- Lines 59-67: the gate-complexity comparison is helpful, but "always improves the `L`-dependence" should be qualified by the theorem assumptions and by which term in the max dominates.
- Lines 73-79: the mixing lemma explanation is strong and worth keeping.
- Lines 95-99: caveats are good. The "does not exploit locality" caveat should mention that randomization is complementary to locality and can sometimes be combined with structure-aware analyses.

## Missing or Suggested Additions

- Add one sentence distinguishing randomized product formulas, qDRIFT, and random compiler/noise-tailoring usage of "randomization"; the terms are easy to conflate.
