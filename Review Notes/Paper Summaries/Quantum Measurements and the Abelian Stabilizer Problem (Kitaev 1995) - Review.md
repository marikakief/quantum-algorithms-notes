# Review: Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995)

Source note: [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]

## Primary Sources Checked

- Kitaev, "Quantum measurements and the Abelian Stabilizer Problem", arXiv: https://arxiv.org/abs/quant-ph/9511026
- ArXiv abstract confirms: polynomial algorithm for Abelian stabilizer including factoring/discrete log; eigenvalue-measurement procedure; QFT for arbitrary finite abelian groups.

## Verdict

Minor issues. The note captures the right paper-level contributions, but some theorem-level cost statements need source-precise phrasing.

## Line-Anchored Comments

- Lines 9-15: paper contributions

  Correct. The arXiv abstract explicitly says the method is based on measuring eigenvalues of a unitary and also yields a polynomial QFT for arbitrary finite abelian groups.

- Line 40: "Cost: O(log(1/epsilon)) controlled-U applications for fixed precision"

  This is only for a fixed-precision single-bit/statistical estimate of trigonometric quantities with confidence amplification. It should not be read as the cost of `l`-bit phase estimation. Add "for constant additive precision in the single-bit cosine/sine experiment."

- Lines 44-48: multi-bit measurement / Theorem 1

  These lines compress Kitaev's original notation heavily. The statement should clarify what is counted as an "operation" and what access is assumed to the controlled-power operator `U^[0,r]`. Otherwise readers may confuse this with the later textbook QPE cost model using controlled `U^{2^j}` gates.

- Lines 57-58: factoring and discrete log as stabilizers

  Good, but write the actions more carefully. The factoring/order-finding action should not reuse `g` for both a group element and exponent in a way that hides the cyclic-period structure.

- Lines 90-96 and 122: compute-copy-uncompute

  The note is useful, but it slightly blurs two constructions: reversible evaluation of a function as `(u,v) -> (u, v xor F(u))`, and garbage-free implementation of a bijection using `G` and `G^{-1}`. Keep Lemma 1 and Lemma 2 separated in the reusable-ideas section.

- Lines 132-143: duplicate references

  Simon, Shor, and Deutsch are each listed twice. Collapse duplicate reference entries.

## Missing or Suggested Additions

- Add a small "Kitaev QPE versus textbook QPE" note: Kitaev estimates eigenphases through repeated controlled-unitary measurement procedures; Cleve-Ekert-Macchiavello-Mosca popularize the inverse-QFT circuit presentation.
