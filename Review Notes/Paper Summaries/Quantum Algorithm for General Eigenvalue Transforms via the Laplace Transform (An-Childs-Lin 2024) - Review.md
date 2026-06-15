# Review: Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024)

Source note: [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]]

## Primary Sources Checked

- An, Childs, and Lin, "Laplace Transform-Based Quantum Eigenvalue Transformation via Linear Combination of Hamiltonian Simulation", SIAM Journal on Computing 2026 / arXiv:2411.04010.

## Verdict

Major issues. The conceptual comparison to QEVT and QSVT is useful, but there are several formula-level errors in the kernel and fractional-inverse sections, plus a misleading comparison table for original LCHS.

## Line-Anchored Comments

- Lines 17-25: good conceptual positioning. Lap-LCHS is indeed a different mechanism from QEVT and singular-value QSVT.
- Line 33: `H^1(C^-)` is described as the Hardy space of the left half-plane. In the LCHS notes, `C^-` denotes the lower half-plane in the complex `k` variable. Check the paper's notation and do not call it the left half-plane unless that is actually the convention used there.
- Line 37: the kernel normalization appears wrong. Nearby notes use `f_beta(k)=C_beta^{-1} exp((1+ik)^beta)` with `C_beta=2 pi exp(-2^beta)`. This line has `(1/2 pi) exp(-2 beta)`, which is a different constant and likely a typo.
- Lines 47-51: "Riemann sums (or higher-order quadrature)" is too generic for a theorem-level summary. The complexity depends on the quadrature assumptions and smoothness/tail bounds of `g`.
- Lines 66-78: theorem summary is too compressed. The `K` and `T` truncation parameters come from different tails (`f` and `g`), so they should not be collapsed into a generic near-linear expression without stating how they are chosen.
- Lines 98-100: the fractional-power inverse query formula has the wrong dependence on `eta`. The denominator `eta^{-(p+1)} ||x||` would make the cost shrink as `eta -> 0`, which is the opposite of the conditioning expected for shifted inverse powers. Recheck Corollary 9.
- Lines 114-118: the comparison table misstates original LCHS. Original Cauchy-kernel LCHS has polynomial/`1/epsilon` precision scaling in the general setting; the polylogarithmic precision improvement belongs to the Hardy-kernel follow-up and Lap-LCHS variants under stated assumptions.
- Line 126: "dissipativity is non-negotiable" should be softened to "the representation requires a positive semidefinite Hermitian part, possibly after a shift whose cost must be paid." A shift can change the normalization/amplification burden.

## Missing or Suggested Additions

- Add exact bibliographic data for the SIAM version if this note is meant to supersede the arXiv-only title.
- Include one worked transform with correct `g(t)`, `||g||_1`, truncation `T`, and resulting query complexity. That would prevent the current table from hiding the hard part of the theorem.
