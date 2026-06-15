# Review: Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Hoyer-Sanders 2010)

Source note: [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/0812.0562

## Verdict

Major issues in one theorem transcription; otherwise a useful summary. The main conceptual story is right: Suzuki formulas extend to ordered exponentials under finite smoothness assumptions. However, the displayed bound for `Q_k` is impossible as written and should be rechecked against the paper before this note is used as a reference.

## Line-Anchored Comments

- Lines 21-28: The framing is good. The paper's novelty is rigorous non-analytic, finite-smoothness error control for ordered exponentials.

- Lines 35-39: The base formula is described as midpoint evaluation. That is right for the symmetric second-order starting point, but make clear that the recursive ordered-exponential formula applies the lower-order approximation over subintervals, not just repeated evaluation at one global midpoint.

- Lines 65-69: The theorem statement is useful, but the constants should be checked carefully. This paper is often cited for asymptotics, but the constants are exactly what distinguish it from a purely formal Suzuki-order statement.

- Lines 75-81: The displayed bound
  `3/2^{1/3^k} <= Q_k <= 2^k/3^k`
  cannot be correct: for `k=1` the lower bound exceeds the upper bound. This is a material transcription error in a theorem-level formula.

- Lines 85-91: "Sub-polynomially in `Delta lambda`" is not the right headline. The optimized smooth case is near-linear in the time/interval parameter, typically written as a linear factor times subpolynomial overhead in the accuracy/time ratio.

- Line 110: "The smoothness condition is necessary" is too strong as written. What is necessary for this proof and this family of bounds is `2k` bounded derivatives; the paper is not proving a universal impossibility theorem for all algorithms or all integrator designs with lower smoothness.

## Missing or Suggested Additions

- Add a short comparison with the companion 2011 algorithmic paper: this paper gives ordered-exponential decomposition bounds, while the later paper accounts for sparse oracle precision and adaptive timesteps.

- State explicitly that the formulas approximate ordered exponentials by products of ordinary exponentials but still assume each simple exponential is efficiently implementable.

