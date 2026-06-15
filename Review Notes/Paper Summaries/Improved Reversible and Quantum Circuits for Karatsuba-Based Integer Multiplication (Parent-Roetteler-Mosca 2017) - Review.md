# Review: Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017)

Source note: [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]]

## Primary Sources Checked

- Parent, Roetteler, and Mosca, "Improved reversible and quantum circuits for Karatsuba-based integer multiplication", arXiv:1706.03419 / TQC 2017.

## Verdict

Major issues. The conceptual explanation is good, but the displayed exponent formula for the space bound is mathematically inconsistent with the claimed `1.427` exponent.

## Line-Anchored Comments

- Lines 17-21: the space/depth/volume tradeoff is correctly described at a high level.
- Lines 27-33: the Karatsuba decomposition is clear.
- Lines 48-52: good explanation of why naive reversible recursion inflates space.
- Lines 60-65: serious formula problem. The displayed exponent
  `3/2 * log_2(3) / (2 log_2(3) - 1)` evaluates to about `1.095`, not `1.427`. The later general formula on line 83 gives the right Karatsuba exponent, `1 + (log_2 3 - 1)/(2 - log_3 2) ≈ 1.427`. Replace line 64 with the correct expression.
- Lines 66-69: the adaptive cutoff discussion is useful and appropriately empirical.
- Line 81: the concrete `n=400` Toffoli comparison is helpful, but label whether these are compute-only or full compute/uncompute counts.
- Line 94: the `n=2048` ratio looks off. `2048^(1.585-1.427)` is about `3.3`, not `4.6`. Recompute or specify which constants are included.
- Lines 98-104: the caveats are good and should remain.

## Missing or Suggested Additions

- Add one sentence clarifying that this is general quantum multiplication of two quantum inputs, whereas Shor modular multiplication often multiplies by classical constants and has different constants.

