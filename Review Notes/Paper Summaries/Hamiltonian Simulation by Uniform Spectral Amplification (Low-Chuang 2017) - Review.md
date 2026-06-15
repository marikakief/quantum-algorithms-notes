# Review: Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017)

Source note: [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]]

## Primary Sources Checked

- Low and Chuang, "Hamiltonian Simulation by Uniform Spectral Amplification", arXiv:1707.05391.

## Verdict

Major issues. The note is detailed and valuable, but the low-energy-subspace speedup comparison appears inverted: it describes `O(alpha sqrt(Delta))` as an improvement over `O(alpha Delta)`, which cannot be right for `Delta < 1`.

## Line-Anchored Comments

- Lines 7-13: good problem statement. Uniformity is indeed the hard part.
- Lines 21-23: the first item is conceptually good, but the query comparison is suspect. If `Delta` is a fractional low-energy width smaller than 1, then `alpha sqrt(Delta)` is larger than `alpha Delta`; it cannot be a speedup over `alpha Delta`. Recheck the baseline. The likely comparison is against full normalization `alpha`, or against a naive amplification cost that is worse in `Delta`.
- Lines 23-25: good summary of amplitude multiplication and standard-form universality.
- Lines 35-40: the flexible-QSP and low-energy polynomial discussion is clear.
- Lines 57-67: theorem statements are helpful. The low-energy corollary should define precisely whether `Delta` is a window width, a relative gap, or an effective normalization target.
- Lines 68-71: Theorem 5 and lower bound are useful. Make the lower-bound parameters match the table notation: `s`, `||H||_1`, and `||H||_max` should not float independently.
- Lines 83-88: the comparison to QSP is mostly good, but the phrase "improvement over QSP" should be qualified as an improvement under extra factorized row/column structure, not a worst-case improvement over optimal simulation.
- Lines 94-99: the caveats are good and should be moved nearer the top because this paper is structure-sensitive.

## Missing or Suggested Additions

- Add a one-paragraph glossary of `alpha`, `Lambda`, `Delta`, `||H||_1`, and `||H||_max`. This note has too many normalization parameters for a reader to infer them.
