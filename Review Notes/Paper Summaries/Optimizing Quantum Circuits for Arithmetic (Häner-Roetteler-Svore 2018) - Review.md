# Review: Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018)

Source note: [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]]

## Primary Sources Checked

- Häner, Roetteler, and Svore, "Optimizing quantum circuits for arithmetic", arXiv:1805.12445 (2018).

## Verdict

Minor issues. This is a detailed and mostly reliable note. The main issue is wording around "parallel" polynomial evaluation and compute-only counts.

## Line-Anchored Comments

- Lines 19-25: the two-technique summary is good.
- Lines 35-40: "evaluate all `M` polynomials in parallel" can mislead. The circuit uses a label register and controlled coefficient selection to evaluate the selected polynomial coherently; it does not compute all `M` polynomial values into separate output registers.
- Lines 43-49: the Toffoli/qubit formulas are useful, but they depend on the paper's fixed-point conventions. Add a pointer to the sign/binary-point assumptions.
- Lines 57-73: the inverse-square-root Newton section is clear and valuable.
- Lines 101-111: the table should say "compute only" before the table, not only after it. Many readers will copy the Toffoli counts without doubling for uncomputation.
- Lines 136-146: the caveats are excellent, especially multiplication dominance and later state-preparation alternatives.

## Missing or Suggested Additions

- Add a small comparison of arithmetic evaluation versus QROM interpolation. The note mentions it, but the choice is now central in modern resource estimates.

