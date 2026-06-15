# Review: Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023)

Source note: [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]

## Primary Sources Checked

- Costa, Schleich, Morales, and Berry, "Further improving quantum algorithms for nonlinear differential equations via higher-order methods and rescaling", npj Quantum Information 2025 / arXiv:2312.09518.

## Verdict

Major issues. The technical summary is strong, especially the rescaling discussion. The largest problem is misuse of complexity-class language: the Liu et al. large-`R` result is a query lower bound/intractability result, not a statement that the problem is "BQP-hard."

## Line-Anchored Comments

- Lines 29-41: good concise summary of the three improvements. The rescaling item is especially clear.
- Line 31: "instead of near-linear" should say "instead of near-linear in `1/epsilon`" or "instead of polynomial/linear precision dependence." As written, it is easy to confuse with time scaling.
- Lines 61-69: this is a good explanation of the rescaling tradeoff and the amplitude-amplification factor.
- Line 75: "double-log structure" is misleading. The displayed complexity has a product of logarithmic factors, not necessarily a `log log` dependence.
- Lines 83-85 and 120-124: excellent caveat about max-norm PDE stability versus 2-norm quantum rescaling.
- Line 108: "Krovi ... unclear rescaling" needs more precision. If the source comparison is not fully checked, phrase as "does not include the vector-rescaling improvement of this paper."
- Line 120: replace "BQP-hard to solve" with "requires exponential query complexity in the lower-bound oracle model" or similar. BQP-hard means at least as hard as all BQP problems; it does not mean "hard for BQP."
- Line 126: good no-driving-term caveat; it belongs in the problem statement too.
- Line 130: "cannot handle genuine qualitative nonlinear behaviour" is too broad. The proven efficient regime is weakly nonlinear/strongly dissipative and excludes many such regimes; the method itself is not categorically incapable of finite-time approximation outside the theorem.
- Line 148: says Berry-Costa is "not yet in the vault", but the vault has [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]].

## Missing or Suggested Additions

- State whether the cited `N` in the complexity is the Carleman truncation order. This is a recurring ambiguity in the nonlinear-DE notes.
