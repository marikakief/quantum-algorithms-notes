# Review: Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015)

Source note: [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes]]

## Verdict

Minor issues. The note is thorough and unusually good about errata and SPAM caveats. Only a few precision and model caveats need tightening.

## Comments

- The `1/sqrt(8)` additive-error threshold is central, but readers should not confuse it with a physical gate-error threshold. It is a sufficient condition for the robust phase-estimation analysis.

- The protocol assumes systematic coherent errors can be amplified by repeated gate application before decoherence destroys the signal. This coherence-limited condition should be tied to the Heisenberg-scaling claim.

- The sequential structure matters: estimating theta uses a corrected or sufficiently bounded Z gate. The note says this; keep it visible in any shortened version.

- SPAM errors are treated as bounded additive probability errors. More adversarial, time-correlated, leakage, or drift errors are outside the simple theorem.

- The errata paragraph is valuable. If the original note is edited, date the corrected constants and cite the follow-up analyses.

## Suggested Additions

- Add a small table mapping alpha, epsilon, and theta to the experiment used and the required prior bound.
- Add "single-qubit only" to the headline limitations.
