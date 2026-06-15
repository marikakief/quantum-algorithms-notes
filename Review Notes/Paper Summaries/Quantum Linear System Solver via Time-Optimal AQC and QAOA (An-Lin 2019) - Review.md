# Review: Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019)

Source note: [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]

## Primary Sources Checked

- An and Lin, "Quantum linear system solver based on time-optimal adiabatic quantum computing and quantum approximate optimization algorithm", ACM TQC 2022 / arXiv:1909.05500.

## Verdict

Major issues. The conceptual summary is good, but two formulas are wrong or at least written in a way that cannot be correct: the AQC(exp) error bound decreases in the wrong direction with runtime, and the QAOA angles divide by the time step instead of multiplying by it.

## Line-Anchored Comments

- Lines 15-22: good high-level summary of AQC(p), AQC(exp), and the QAOA connection.
- Lines 48-56: the power-law schedule and `O(kappa/epsilon)` scaling are clear.
- Line 68: the exponential error formula is dimensionally and qualitatively wrong as written. `exp[-C' (kappa log^2 kappa * T)^(-1/4)]` tends to 1 as `T` grows, so it cannot be the final error bound. It should have runtime in the numerator of the exponent, e.g. a form like `exp[-c (T/(kappa polylog kappa))^{1/4}]`, depending on the exact theorem.
- Line 70: the runtime conclusion may be right, but it currently follows from a bad displayed formula. Fix the formula first.
- Lines 80-92: the QAOA discussion is useful, but line 88 has the time-step factor inverted. A first-order Trotter slice with duration `Delta t = T/P` should give angles proportional to `(1-f(s_j)) Delta t` and `f(s_j) Delta t`, not divided by `Delta t`.
- Line 92: "using AQC(exp) parameters as initial guess" blurs a theorem with a variational heuristic. The rigorous guarantee is from taking Trotterized AQC-derived parameters; optimization can improve numerically but is not the theorem.
- Lines 112 and 125: good caveats about the QAOA guarantee being weak.
- Line 110 and 126: if this is revised, say Costa et al. removes the residual `log kappa` overhead and achieves optimal `O(kappa log(1/epsilon))`, not just generic `polylog(kappa/epsilon)`.

## Missing or Suggested Additions

- Add exact theorem statements for AQC(exp) and QAOA from the paper. The note is currently most fragile precisely where the formulas matter.

