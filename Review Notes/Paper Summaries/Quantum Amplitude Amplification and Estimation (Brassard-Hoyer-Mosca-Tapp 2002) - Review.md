# Review: Quantum Amplitude Amplification and Estimation (Brassard-Hoyer-Mosca-Tapp 2002)

Source note: [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]

## Primary Sources Checked

- BHMT, "Quantum Amplitude Amplification and Estimation", arXiv: https://arxiv.org/abs/quant-ph/0005055
- AMS Contemporary Mathematics DOI listed by arXiv: https://doi.org/10.1090/conm/305/05215

## Verdict

Major issues. The note has the right high-level account, but it incorrectly states certainty for the basic known-`a` algorithm.

## Line-Anchored Comments

- Lines 11-13: headline definitions

  Good. The arXiv abstract explicitly says amplitude amplification gives expected applications proportional to `1/sqrt(a)` assuming `A` makes no measurements, and amplitude estimation estimates `a`.

- Lines 45-47: "success probability >= 1-a" versus "found with certainty"

  Contradiction. Standard amplitude amplification with the usual `pi` reflections and `m = floor(pi/(4 theta_a))` does not generally succeed with certainty; it succeeds with high probability bounded as stated at line 45. Certainty requires exact amplitude amplification using adjusted phases/iterations under additional knowledge. Rewrite Theorem 2 accordingly.

- Line 49: unknown `a`

  Good, but say expected applications of `A` and `A^{-1}`, and include the constant/probability qualification only if quoting the theorem.

- Lines 61-67: amplitude-estimation theorem

  Correct standard form for the `8/pi^2`-probability error bound, assuming `M` phase-estimation samples/applications as in BHMT.

- Lines 75-79: approximate/exact counting table

  Needs endpoint care. Exact counting bounds in BHMT are usually written with endpoint-safe terms; bare `O(sqrt(t(N-t)))` fails to communicate what happens when `t=0` or `t=N`.

- Lines 86-93 and 114: "any classical randomized algorithm"

  Too broad. The speedup is for search heuristics that can be coherently implemented with a checkable success predicate. It does not automatically speed up arbitrary adaptive classical computation with irreversible measurements and data-dependent stopping without accounting for reversible simulation.

## Missing or Suggested Additions

- Add a small "ordinary AA vs exact AA" subsection so the certainty claim is not confused with the usual Grover iterate.
