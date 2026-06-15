# Review: Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan-Wiebe-Babbush-Aspuru-Guzik 2017)

Source note: [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]

## Primary Sources Checked

- Kivlichan, Wiebe, Babbush, Aspuru-Guzik, "Bounding the costs of quantum simulation of many-body physics in real space," J. Phys. A 50, 305301 (2017) / arXiv:1608.05696.

## Verdict

Major issues. The note understands the paper's layered error accounting, but it overstates the worst-case discretization consequence and gets the Coulomb-regularization direction muddled.

## Line-Anchored Comments

- Lines 17-21: good cost-model warning. The note correctly says the result is query-style and treats state preparation/classical logic as outside the main cost.
- Lines 35-39: the finite-difference coefficient 1-norm point is important and accurately highlighted.
- Lines 49-59: the LCU decomposition summary is useful, but define `delta` and `M` more carefully; these are approximation/discretization parameters, not arbitrary scaling knobs.
- Lines 106-120: the discretization warning is central, but line 110 says the complexity can be "doubly-exponential" in `eta D`. From the displayed `h` dependence, the grid-cost blowup is exponential in `eta D`, not double exponential, unless an additional exponentially small parameter is being folded in.
- Lines 138-140: excellent distinction between discrete-Hamiltonian simulation error and continuous-discretization error. This distinction should be moved earlier because it is the paper's main lesson.
- Line 148: regularizing `1/r` with `sqrt(r^2 + Delta^2)` introduces an error that decreases as `Delta` is taken small, while the potential norm grows like `1/Delta`. The note currently phrases the regularization error as `q^2/Delta`, which has the wrong monotonicity for an approximation-error statement.

## Missing or Suggested Additions

- Add a short table separating `h`, `a`, `Delta`, `V_max`, and the finite-difference/order parameters. The current note is technically rich but parameter-dense.
