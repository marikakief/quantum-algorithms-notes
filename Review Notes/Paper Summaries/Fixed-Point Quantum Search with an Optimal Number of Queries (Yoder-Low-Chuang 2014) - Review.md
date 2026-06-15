# Review: Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014)

Source note: [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes]]

## Primary Sources Checked

- Yoder, Low, Chuang, "Fixed-point quantum search with an optimal number of queries", arXiv: https://arxiv.org/abs/1409.3305
- PRL DOI listed by arXiv: https://doi.org/10.1103/PhysRevLett.113.210501

## Verdict

Minor issues. Strong note overall, with a few claims that need softening or source-specific wording.

## Line-Anchored Comments

- Lines 17-19: "first fixed-point AA retaining optimal scaling" and QSP lineage

  The first claim matches the arXiv abstract. The QSP lineage is reasonable but should be phrased as "a direct precursor to the Low-Chuang QSP program" rather than "seeded the entire program" unless sourced.

- Lines 45-46: success-probability formula

  The formula presentation is hard to parse and may be algebraically malformed. Use the exact notation from the paper, defining `gamma` and the argument of `T_L` clearly. This is a high-risk formula because later trick cards quote it.

- Lines 57-60: special cases

  `delta = 1` recovering standard Grover is plausible. `delta = 0` is a singular limit in the displayed definitions; describe Grover's `pi/3` algorithm as a limiting/special construction only if the paper states it that way.

- Lines 69-73: nesting/adaptivity

  Good idea, but "check if result is good enough" may require measurement and restart, which conflicts with "without restarting" if read literally. Clarify coherent nesting versus experimental/adaptive stopping.

- Lines 97-103: comparison table

  The "AA + estimation" row is too vague. Amplitude estimation can learn the overlap and then enable ordinary AA, but the listed `log log` factor needs a source or should be removed.

## Missing or Suggested Additions

- Add the fixed-point lower-bound statement from the paper: optimality is for algorithms satisfying the fixed-point property over a range of overlaps, not for all possible search algorithms.
