# Review: Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021)

Source note: [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]

## Primary Sources Checked

- Tong, An, Wiebe, and Lin, "Fast inversion, preconditioned quantum linear system solvers, fast Green's-function computation, and fast evaluation of matrix functions", PRA 2021 / arXiv:2008.13295.

## Verdict

Major issue in the matrix-functions section. The preconditioned QLSP and Green's-function parts are strong, but the statement that `f(A+B)=g(W)` for `W=I+A^{-1}B` is not generally true for noncommuting matrices. That needs to be checked against the paper's exact transform construction.

## Line-Anchored Comments

- Add a top-level title.
- Lines 9-21: good setup and conceptual framing of fast inversion as quantum preconditioning.
- Lines 29-39: the fast-inversion primitive is clear. However, line 37 should say the query count is independent of `kappa(D)`; the block-encoding normalization `alpha'_A=||D^{-1}||` still affects later success/amplification costs.
- Lines 43-52: good preconditioned QLSP construction and correct use of the state-dependent `xi`.
- Lines 54-65: the Green's-function application is well summarized.
- Lines 67-79: revise carefully. The contour-integral route is plausible as written, but line 77's "note that if `A` is fast-invertible, then `f(A+B)=g(W)`" is mathematically suspect unless there are commutation assumptions or a more specific transform. In general `A+B = A(I+A^{-1}B)`, and a matrix function of a product is not a function only of the second factor.
- Lines 85-95: theorem statements are useful. Keep the `tilde{sigma}_{min}` and `xi` caveats attached to them.
- Lines 118-123: strong caveats. These are essential because the algorithm is advantageous only under structural splitting assumptions.

## Missing or Suggested Additions

- Replace the matrix-functions subsection with the paper's exact theorem statements and assumptions. In particular, state when the inverse-transform/QSVT route applies and what operator is actually being transformed.

