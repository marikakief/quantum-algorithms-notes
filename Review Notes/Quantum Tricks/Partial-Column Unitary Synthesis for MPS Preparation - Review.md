# Review: Partial-Column Unitary Synthesis for MPS Preparation

Source note: [[Partial-Column Unitary Synthesis for MPS Preparation]]

## Primary Sources Checked

- Berry et al., arXiv:2409.11748 / PRX Quantum 6, 020327 (2025).

## Verdict

Minor issues. The card is technically solid. The main improvement is to localize the quoted constant-factor speedups to the exact synthesis task.

## Line-Anchored Comments

- Lines 8-15: the idea that MPS preparation only needs part of a unitary is well explained.
- Lines 28-37: the distinction between full unitary synthesis and partial-column synthesis is good and should stay.
- Line 54: the `7x` Toffoli reduction and `d=4` comparison should be described as subroutine-level synthesis improvements, not as an automatic total resource reduction for end-to-end QPE.
- Lines 60-66: the "not a new asymptotic class" caution is appropriate. Keep it prominent.

## Missing or Suggested Additions

- Add one sentence saying that total state-preparation cost still depends on bond dimension, orbital ordering, target overlap, and any subsequent filtering/search.

