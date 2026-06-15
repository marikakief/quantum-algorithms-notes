# Review: High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014)

Source note: [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]

## Primary Sources Checked

- Berry, "High-order quantum algorithm for solving linear differential equations", J. Phys. A 2014 / arXiv:1010.2745.

## Verdict

Major scope issue. The history-state and multistep-method explanation is good, but the note is internally inconsistent about time-dependent coefficients and mislabels a later QLSP paper as an "optimal ODE algorithm."

## Line-Anchored Comments

- Add a top-level title.
- Lines 7-13: the problem statement uses `A(t)` and `b(t)`, suggesting a rigorous time-dependent-coefficient algorithm.
- Line 108: the caveat says the rigorous result is constant coefficients only and time dependence is informal. This contradicts the opening problem statement. Put the coefficient restrictions in the problem statement.
- Lines 15-21: the high-level contribution is good once the coefficient scope is fixed.
- Lines 31-42: the history-state linear system construction is well explained.
- Lines 45-59: good explanation of why high-order multistep methods improve the time-step count and how the condition number enters.
- Lines 67-71: the result table is useful.
- Line 87: "Costa, An, Sanders, Su, Babbush, Berry (2022) - optimal ODE algorithm" is wrong. That paper is an optimal QLSP solver, not an ODE solver. If the intended successor is Berry-Costa 2022 for time-dependent differential equations, cite that separately and accurately.
- Lines 93-99: the comparison table's "Costa et al. (2022)" column should be removed or renamed to the correct ODE paper. It currently mixes QLSP and ODE solver lineages.
- Lines 102-110: good caveats; move the constant-coefficient caveat to the top.

## Missing or Suggested Additions

- Add a short theorem statement with all promises: coefficient model, diagonalizability/stability assumptions, `g` or final-state norm assumptions, and output success probability.

