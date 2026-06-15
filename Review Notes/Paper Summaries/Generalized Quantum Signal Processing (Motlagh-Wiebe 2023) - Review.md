# Review: Generalized Quantum Signal Processing (Motlagh-Wiebe 2023)

Source note: [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]]

## Primary Sources Checked

- Motlagh and Wiebe, "Generalized Quantum Signal Processing", PRX Quantum 2024 / arXiv:2308.01501.

## Verdict

Minor issues. This is a detailed and mostly accurate note. The main corrections are to avoid overstating how completely GQSP removes restrictions in contexts outside unitary polynomial transformations.

## Line-Anchored Comments

- Lines 9-13: good problem statement. It should specify that this is polynomial transformation of a unitary, not singular-value transformation of an arbitrary block-encoded matrix.
- Lines 19-24: the high-level contribution is right. "The only requirement left" should be read in the unitary/Laurent-polynomial setting; QSVT-style nonunitary block-encoding has additional structure.
- Lines 34-38: good contrast between 0-controlled `U` and standard QSP signal operators.
- Lines 50-58: the theorem statement is clear and useful.
- Lines 71-75: good Riesz-Fejer/factorization explanation.
- Lines 97-99: the fractional-query and square-root phase results are good, but define `delta` as a spectral gap/eigenphase lower bound more explicitly. `sigma_min(H)` can be misleading if `H` is a generator modulo phases.
- Lines 116-117: the `10^4` versus `10^7` phase-finding comparison is empirical/software-dependent. Mark it as an implementation result, not a theorem-level separation.
- Lines 129-137: the caveats are good. The "No QSVT generalisation here" caveat is important and should appear earlier because many readers will assume GQSP immediately generalizes QSVT.
- Line 198: a 2026 paper cannot be a reference or stable "later connection" unless this note is actively maintained. Mark it as a future/current vault cross-link, not part of this paper's context.

## Missing or Suggested Additions

- Add a small table separating standard QSP on `[-1,1]`, Laurent/unitary QSP on the unit circle, GQSP, and QSVT. That would prevent conflating their polynomial constraints.
