# Review: Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023)

Source note: [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes]]

## Primary Sources Checked

- An, Liu, and Lin, "Linear combination of Hamiltonian simulation for nonunitary dynamics with optimal state preparation cost", PRL 2023 / arXiv:2303.01029.

## Verdict

Minor issues. The note explains the Cauchy-kernel LCHS idea well and correctly identifies the precision bottleneck that motivated the later Hardy-kernel paper. Most fixes are precision around implementation claims.

## Line-Anchored Comments

- Lines 25-33: excellent high-level description. The Cauchy-tail `1/epsilon` bottleneck is the right takeaway.
- Line 31: cite the lower bound more concretely if possible. "An-Liu-Wang-Zhao 2022" is not enough for a reader to locate the result.
- Lines 43-55: the contour proof sketch is helpful but delicate. If retained, make clear which complex variable is being integrated and which half-plane is used; the current `omega=ik` explanation can be hard to follow.
- Lines 59-65: the discrete LCU implementation is clear. For signed/complex coefficients in related kernels, PREPARE needs phases, not just square roots of positive `c_j`; the Cauchy case is positive.
- Lines 81-82: the hybrid-observable query statement should be checked. It mixes classical sampling overhead and quantum observable-estimation cost in one sentence.
- Lines 85-89: good interaction-picture caveat. This special fast-forwardable-`L` case should be separated from the general theorem because it changes the practical impact of the `K=O(1/epsilon)` cutoff.
- Lines 95-105: theorem summary is useful. Define `Gamma_p` in words as a smoothness/derivative parameter before giving the formula.
- Line 148: "QSVT ... which LCHS explicitly avoids" is too glib. LCHS avoids QLSA-style linear-system solving in the main construction; Hamiltonian-simulation subroutines may still be implemented with QSP/QSVT machinery.

## Missing or Suggested Additions

- Add a one-line explanation of how the inhomogeneous term changes the state-preparation ratio from `||u_0||/||u(T)||` to `(||u_0||+||b||_{L^1})/||u(T)||`.
