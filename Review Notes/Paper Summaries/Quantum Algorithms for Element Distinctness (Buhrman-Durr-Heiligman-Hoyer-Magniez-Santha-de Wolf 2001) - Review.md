# Review: Quantum Algorithms for Element Distinctness (Buhrman-Durr-Heiligman-Hoyer-Magniez-Santha-de Wolf 2001)

Source note: [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]]

## Primary Sources Checked

- Buhrman et al., "Quantum Algorithms for Element Distinctness", arXiv: https://arxiv.org/abs/quant-ph/0007016
- SIAM journal DOI listed by arXiv: https://doi.org/10.1137/S0097539702402780

## Verdict

Minor issues. The note accurately captures the nested-amplification algorithm and its historical role. A few comparison rows and links need tightening.

## Line-Anchored Comments

- Line 13: comparison model

  Correct that the paper is explicitly in the comparison-complexity model. The "function-evaluation model too" add-on should be explained: if an oracle returns values, comparisons and sorting have a different accounting than in a pure comparison oracle.

- Lines 19-21: headline algorithm

  Good source match. The arXiv abstract states an `O(N^{3/4} log N)` quantum upper bound for element distinctness and an `Omega(N^{1/2})` comparison lower bound.

- Lines 31-39: claw-finder cost

  The structure is right: random subsets, sort one side, Grover/binary-search over the other side, then outer amplitude amplification.

- Lines 49-51 and 78-79: BHT comparison

  Be careful calling this an "ED bound". BHT solves collision finding under a strong `r`-to-one promise. In the table, make the row visibly separate from ordinary element distinctness; otherwise it reads like an `O(N^{1/3})` ED algorithm.

- Lines 57-65: Theorem labels

  Mostly good. If the note keeps theorem numbers, verify them against the final SIAM version or explicitly say they are from the arXiv/full version; theorem numbering can drift between versions.

- Lines 90-99: lower bounds

  Correct. The paper's own lower bound is only `Omega(sqrt(N))`; the tight `Omega(N^{2/3})` lower bound is later Shi/Aaronson-Shi polynomial-method work.

- Lines 135 and 162-style links

  Some links point to a broader amplitude-amplification note when the cited result is specifically Boyer-Brassard-Hoyer-Tapp search with unknown number of marked items. Link either to `[[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]` or explain why BHMT is being used for the later framework.

## Missing or Suggested Additions

- Add a one-sentence explanation that this algorithm's `log N` factor comes from sorting/binary search in the comparison model, while Ambainis later removes the repeated setup by walking between adjacent subsets.
