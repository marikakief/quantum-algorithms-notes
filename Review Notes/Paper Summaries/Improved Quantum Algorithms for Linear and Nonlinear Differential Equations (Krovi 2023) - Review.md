# Review: Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023)

Source note: [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]]

## Primary Sources Checked

- Krovi, "Improved quantum algorithms for linear and nonlinear differential equations", Quantum 2023 / arXiv:2202.01054.

## Verdict

Minor issues. The note correctly identifies `C(A)=sup_t ||e^{At}||` and log-norm stability as the conceptual contributions. A few formulas and assumptions need tightening.

## Line-Anchored Comments

- Lines 11 and 19-20: the opening says the linear ODE assumes all eigenvalues have negative real part, but the later paragraph says the algorithm extends to singular/non-diagonalizable cases when `C(A)` is finite. Reconcile these by separating the basic stable setting from the general bounded-propagator analysis.
- Lines 21 and 27: "exponentially improves error dependence" is fair relative to forward Euler, but spell out that the improvement comes from the time-discretisation method, not from Carleman linearisation itself.
- Line 53: the displayed log-norm expression has a parenthesis/notation problem. It should be about the logarithmic norm of `DAD^{-1}`, i.e. the largest eigenvalue of `(B+B^\dagger)/2` for `B=DAD^{-1}`, not `mu(B+B^\dagger)/2`.
- Lines 57-59: good explanation of why log norm is stricter than spectral abscissa and handles non-normality honestly.
- Line 68: the Carleman truncation-order expression contains `log(1/||u_in||)` in the denominator. This only makes sense after a scaling regime with `||u_in||<1`; mention the rescaling/normalization assumption right in the table.
- Lines 84-86 and 92: the measurement-penalty discussion is important, but the notation `N` switches between Carleman truncation order and problem dimension in nearby notes. Use a distinct symbol for the Carleman order to avoid confusion.
- Line 96: "time-independent `F_0` only" is a good caveat. It should be moved closer to the problem statement, because line 13 compares directly to Liu et al.'s time-dependent forcing.

## Missing or Suggested Additions

- Include one concrete example of a non-normal stable matrix with large transient growth to illustrate why `C(A)` is the right quantity and why it can still be large.
