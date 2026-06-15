# Review: Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023)

Source note: [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]]

## Primary Sources Checked

- Schmidhuber and Lloyd, "Complexity-theoretic limitations on quantum algorithms for topological data analysis", arXiv:2209.14286 / PRX Quantum 4, 040349 (2023).

## Verdict

Minor issues. The negative-result story is accurate. The main concern is that some "oracle model recovers advantage" language is broader than the theorem warrants.

## Line-Anchored Comments

- Lines 16-23: good summary of the exact and decision hardness results.
- Line 19: multiplicative approximation hardness follows through the zero-versus-positive decision issue; spell that out because multiplicative error at zero is a common source of confusion.
- Lines 31-39: the Euler-characteristic reduction is well presented.
- Lines 45-53: the random Vietoris-Rips discussion is useful, but the lower-bound formula should mention the assumptions on simplex count and Betti-number scaling.
- Lines 76-80: "oracle model recovers advantage" is too broad. Uniform simplex-sampling access removes one bottleneck, but advantage still depends on clique fraction, spectral gap, target error, and Betti-number size.
- Line 82: good inclusion of the QMA_1-hardness context. Since it is concurrent/independent, keep it distinct from the PRX Quantum paper's own theorem list.

## Missing or Suggested Additions

- Add one sentence distinguishing worst-case hardness of exact/nonzero Betti problems from the approximate normalized spectral-density problems used in positive TDA algorithms.

