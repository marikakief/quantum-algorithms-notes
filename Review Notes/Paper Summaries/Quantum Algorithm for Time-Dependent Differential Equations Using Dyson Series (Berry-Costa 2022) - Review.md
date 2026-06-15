# Review: Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022)

Source note: [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]]

## Primary Sources Checked

- Berry and Costa, "Quantum algorithm for time-dependent differential equations using Dyson series", Quantum 2024 / arXiv:2212.03544.

## Verdict

Major issues. The architecture is described well, but the note appears to use a wrong error metric and reuses the symbol `R` for two different quantities in a way that corrupts the complexity discussion.

## Line-Anchored Comments

- Line 13: "Bures-Wasserstein metric" is almost certainly wrong for this vector ODE algorithm. The output guarantee should be stated in the paper's norm/state-distance convention for normalized solution vectors, not a density-matrix optimal-transport metric.
- Lines 15 and 118: log-norm nonpositivity is a sufficient stability promise for the theorem, not a general necessity for solving linear ODEs. Avoid saying unstable cases are excluded by the problem itself; they are outside this efficient guarantee.
- Lines 19-23: good synthesis of history-state encoding with Dyson-series block-encodings.
- Lines 38-48: the block-bidiagonal linear system description is clear.
- Line 48: `R=2r` is introduced as the total number of rows/segments.
- Lines 78-87: `R` now denotes the amplitude-amplification/normalization factor. This symbol collision is serious because it changes how a reader interprets the theorem. Use different symbols, e.g. `R_steps` and `R_amp`.
- Lines 85-87: good that `D` is confined to gate complexity, but define whether it is a derivative bound, a discretization parameter, or both.
- Lines 97 and 122: "no amplitude amplification needed for state preparation" conflicts with the immediately stated `R = O(x_max/||x(T)||)` overhead. Clarify whether this means no repeated initial-state preparation inside the QLSA, or no final postselection amplification.
- Lines 101-107: the comparison table is useful, but the BCOW row should say "time-independent `A`" rather than simply "constant `A`" if the inhomogeneous term is allowed.

## Missing or Suggested Additions

- Give the exact theorem's definitions of `lambda_A`, `lambda_Ax`, `x_max`, and the amplification factor. This paper's value is in its parameter accounting, so the notation has to be stable.
