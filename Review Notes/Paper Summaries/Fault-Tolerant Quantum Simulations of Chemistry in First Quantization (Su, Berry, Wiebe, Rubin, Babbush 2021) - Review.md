# Review: Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021)

Source note: [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]

## Primary Sources Checked

- Su, Berry, Wiebe, Rubin, Babbush, "Fault-Tolerant Quantum Simulations of Chemistry in First Quantization", arXiv:2105.12767 / PRX Quantum 2, 040332 (2021).
- Cross-checked against the 2019 sublinear-scaling predecessor.

## Verdict

Minor issues. This is a strong summary. Most problems are overbroad phrasing around applicability and a few places where exact normalization details are compressed into intuition.

## Line-Anchored Comments

- Lines 21-25: the framing is accurate and useful. The "first constant-factor resource estimate" claim should remain tied to first-quantized chemistry, not all chemistry simulation.
- Lines 47-50: the failed-PREP fallback explanation is directionally right but simplified. The exact theorem uses adjusted amplitudes, `p_nu`-type success probabilities, and the `P_eq` correction; do not imply the success probability is exactly `1/4`.
- Lines 51-55: good to cite Theorem 4/Eq. 125, but the line about QROM cost `lambda_zeta + Er(lambda_zeta)` is opaque. Define the symbol or avoid it in a student-facing summary.
- Lines 69-72: the qubitized-Dyson explanation is good, but "2/3 the cost" is a constant-factor comparison under the paper's chosen sorting/inequality-test implementations and target eigenvalue-shift regime. Qualify it.
- Lines 103-114: the concrete resource table is one of the strongest parts of the note. It clearly distinguishes first-quantized plane waves from second-quantized Gaussian methods.
- Line 130: "qubitization is usually more practical" is correct for the paper's tested chemistry regimes, but the note should say "in these resource estimates" rather than making a universal claim.
- Line 142: the Slater-determinant preparation comparison is plausible but representation-dependent. Make clear that it concerns the first-quantized preparation method analyzed by the paper, not a lower bound.
- Line 144: "trivially extend to non-Born-Oppenheimer dynamics" overstates the ease. Adding nuclei changes masses, interaction terms, statistics/indistinguishability assumptions, and state-preparation requirements. Say the framework can be extended, but the resource estimates are Born-Oppenheimer.

## Missing or Suggested Additions

- Add a brief note that the paper's headline first-quantized advantage depends on the plane-wave basis and may trade basis-size overhead against qubit savings relative to Gaussian-orbital methods.
- Mention that pseudopotentials are explicitly left for later work; the 2024 pseudopotential note is the continuation.

