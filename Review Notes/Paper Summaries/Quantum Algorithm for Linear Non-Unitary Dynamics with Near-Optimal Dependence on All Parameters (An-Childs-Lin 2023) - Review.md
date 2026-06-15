# Review: Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023)

Source note: [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]

## Primary Sources Checked

- An, Childs, and Lin, "Quantum algorithm for linear non-unitary dynamics with near-optimal dependence on all parameters", CMP 2026 / arXiv:2312.03916.

## Verdict

Minor issues. The note accurately explains the generalized LCHS identity and the Hardy-space kernel improvement. One caveat section heading is misleading, and a few kernel/assumption statements need more precision.

## Line-Anchored Comments

- Lines 21-25: good summary of the paper's contribution: optimal state-preparation queries plus polylogarithmic precision in matrix queries.
- Lines 37-48: the theorem statement is useful. The proof sketch in line 48 should be checked for sign/half-plane language; the contour argument is delicate, and "for `k<0`" is not the right standalone explanation of the decay.
- Lines 52-60: good kernel discussion and good explanation of the Phragmen-Lindelof barrier.
- Lines 64-70: the quadrature parameter description is clear. It would help to distinguish the number of quadrature nodes from Hamiltonian-simulation query complexity, since each node calls a simulation subroutine.
- Lines 102-106: "purified Gibbs state `e^{-gamma L}/Z_gamma`" should be phrased carefully. A purification normally involves square roots/amplitudes; if the paper prepares a purified Gibbs object, state the exact vector or density operator.
- Lines 122-129: good comparison table.
- Line 137: heading says "`T^2` for time-dependent case", but the time-dependent LCHS result in lines 84-94 has linear `alpha_A T` times polylogarithmic precision factors. The `T^2` comparison belongs to time-marching or to certain ODE applications, not this theorem heading.
- Line 135: "Without this, the LCHS identity fails" is a little too absolute. A scalar shift can make a lower-bounded Hermitian part positive at the price of norm/amplification factors. State the assumption in the theorem and then discuss shift costs.
- Lines 164-165: both Berry-Childs-Ostrander-Wang and Childs-Kothari-Somma have vault notes; the "not in vault" comments are outdated.

## Missing or Suggested Additions

- Add the lower-bound statement, if present in the paper, next to the "near-optimal" claim. Otherwise "near-optimal in all parameters" needs a precise reference.
