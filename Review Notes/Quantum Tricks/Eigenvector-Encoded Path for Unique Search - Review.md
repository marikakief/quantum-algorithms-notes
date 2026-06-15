# Review: Eigenvector-Encoded Path for Unique Search

Source note: [[Eigenvector-Encoded Path for Unique Search]]

## Primary Sources Checked

- Montanaro, arXiv:1509.02374: https://arxiv.org/abs/1509.02374

## Verdict

Minor issues. The idea is correct and well explained: under a unique-marked-vertex promise, the eigenvalue-1 state contains the root-to-target path. The main fix is to avoid overprecise dependence on an accuracy parameter without tying it to the theorem.

## Line-Anchored Comments

- Lines 12-16: path eigenvector description is good.

  It assumes the marked vertex is at depth `n` in the displayed normalization. If the actual depth is `ell`, replace `n` in the path normalization by `ell` or explicitly say this is the full-depth case.

- Line 18: `O(sqrt(Tn)/delta^3)` should be checked.

  The final theorem quoted in the paper is usually used as `O(sqrt(Tn) log^3 n log(1/delta))` for the unique-solution variant. If the card keeps the intermediate precision dependence, cite the exact lemma; otherwise remove the `delta^3` claim.

- Line 20: recursion intuition is right.

  Measuring a random path vertex reduces the remaining depth in expectation. The proof is not just Jensen's inequality; it also uses the cost recurrence and success amplification.

- Lines 24-26: good use-case restrictions.

  Emphasize that uniqueness is a promise on marked vertices in the current subtree. If recursive restrictions introduce multiple marked descendants, the argument no longer applies.

## Missing or Suggested Additions

- Add a comparison to the generic search method: binary-searching over child subtrees costs an extra depth factor; the path-eigenvector method replaces that by logarithmic overhead under uniqueness.
