# Review: Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024)

Source note: [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]]

## Primary Sources Checked

- Berry, Motlagh, Pantaleoni, and Wiebe, "Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing", PRA 2024 / arXiv:2401.10321.

## Verdict

Minor issues. The note explains the directional-control idea well. The main fixes are a precision-term convention and one confusing parity sentence.

## Line-Anchored Comments

- Lines 9-17: good access-model setup and good statement of the factor-of-two question.
- Lines 23-27: the high-level assessment is accurate: this is a constant-factor improvement, not a new asymptotic regime.
- Line 25: use the standard precision term when giving complexity: `lambda t + log(1/epsilon)/loglog(1/epsilon)` up to conventions, not just `lambda t + log(1/epsilon)`.
- Lines 33-40: excellent explanation of why directional control doubles the effective Fourier order per query.
- Lines 43-49: good parity-obstruction explanation.
- Lines 63-69: line 69 says "These have only odd powers of `U`"; `P_K` uses even powers and `Q_K` uses odd powers. Rewrite this sentence to say the pair has the parity structure required after the GQSP shift/completion.
- Lines 71-81: the completion procedure is subtle. Check the sign in `1 - alpha^2(P_K^2 - Q_K^2)` and explain why the minus sign appears if `Q_K` is imaginary-valued.
- Lines 95-101: good query-count statement.
- Lines 115-118: again use the `log/loglog` precision convention in the comparison table. A loose `log` hides the point of the optimal simulation literature.
- Lines 128-134: caveats are strong and useful, especially the directional-control assumption and classical preprocessing caveat.

## Missing or Suggested Additions

- Add a sentence distinguishing "queries to the walk/signal unitary" from lower-level block-encoding oracle calls; whether the factor-of-two survives at the physical gate level depends on that implementation.
