# Review: Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024)

Source note: [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]]

## Primary Sources Checked

- Ding, Li, Lin, Ni, Ying, and Zhang, "Quantum Multiple Eigenvalue Gaussian filtered Search: an efficient and versatile quantum phase estimation method", Quantum 2024 / arXiv:2402.01013.

## Verdict

Major issues. The algorithmic story is clear, but the gapped-regime formulas and "constant depth" language need to be rechecked. Some displayed scalings imply impossible behavior as tail weight goes to zero.

## Line-Anchored Comments

- Lines 9-13: good statement of the dominant-overlap condition. It should be described as a signal-to-tail promise, not a replacement for resolution when eigenvalues are arbitrarily close.
- Lines 17-23: clear high-level summary.
- Lines 29-48: the Gaussian-filter construction is explained well.
- Lines 62-70: the gapless theorem summary is plausible and correctly ties total time to `1/epsilon` and the dominant-tail separation.
- Lines 72-76: recheck Theorem 3.2. The displayed `T_max = ~Theta(p_tail/(p_min epsilon))` goes to zero when `p_tail -> 0`, which cannot be a valid longest evolution-time requirement. There should be additional max/gap/log terms or a different parameter convention.
- Lines 78-82: the heading says "constant depth", but the text concludes `T_max = O(log(1/epsilon))` in the favorable case. That is logarithmic depth, not constant depth, unless `epsilon` is fixed.
- Lines 86-96: useful comparison table. Add footnotes for which methods require a spectral gap, dominant-overlap promise, or known number of eigenvalues.
- Line 102: good caveat about grid search. The classical post-processing grid size should also be included in the algorithmic-cost discussion if this note is used for practical comparisons.
- Line 112: "applies even when eigenvalues are arbitrarily close together" is too strong. The gapless theorem gives covering within a width set by `1/T`; it does not necessarily resolve two arbitrarily close dominant eigenvalues as distinct peaks.

## Missing or Suggested Additions

- Define `Delta_dom`, `Delta`, `delta`, and the blocking radius constants before the theorem bullets. The gapped regimes are parameter-sensitive.
