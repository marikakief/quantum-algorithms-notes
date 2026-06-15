# Review: On-the-Fly Coherent Molecular Integral Computation

Source note: [[On-the-Fly Coherent Molecular Integral Computation]]

## Primary Sources Checked

- Babbush et al., *Exponentially more precise quantum simulation of fermions I: Quantum chemistry in second quantization*, arXiv:1506.01020: https://arxiv.org/abs/1506.01020

## Verdict

Minor issues. The card gets the motivation and asymptotic advantage right, but the molecular-integral factorization is much more schematic than the source construction.

## Line-Anchored Comments

- Line 7: The `O(N^4)` to `O(N)` contrast is the intended asymptotic message for the PREPARE step, but it hides precision-dependent arithmetic and normalization factors.

- Lines 11-15: The displayed integral factorization is a useful cartoon, not the actual full Gaussian-integral/Boys-function machinery. Label it as schematic.

- Lines 17-22: Independent preparation of orbital-index registers is the key trick. Add that signs/phases and coefficient normalization must still be handled coherently.

- Line 32: "`O(N)` gates per PREPARE call" should be qualified with `polylog(1/epsilon)` and arithmetic precision costs. For explicit resource estimation, those hidden factors matter.

- Lines 34-38: The caveat is good and should perhaps be moved closer to the complexity line.

## Missing or Suggested Additions

- Add a small normalization note: the LCU coefficient sum controls segment count, not just the cost of preparing indices.
- Mention that this is mainly of historical/asymptotic interest relative to later QROM, low-rank, double-factorization, and THC constructions.
