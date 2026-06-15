# Review: Efficient Bayesian Phase Estimation (Wiebe-Granade 2016)

Source note: [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]]

## Primary Sources Checked

- Wiebe and Granade, "Efficient Bayesian Phase Estimation," Physical Review Letters 117, 010503 (2016) / arXiv:1508.00869.

## Verdict

Minor issues. The note is solid and already includes the important caveats. The main thing to tighten is which claims are empirical rather than proven.

## Line-Anchored Comments

- Lines 7-15: the phase-estimation setup is clear. Check the sign convention in `cos(M(phi + theta))`; many presentations use a minus sign depending on where the feedback phase is applied.
- Lines 19-23: good summary. "Achieves near-Heisenberg-limited scaling" should be labelled empirical/numerical unless paired with a theorem.
- Lines 29-39: the rejection-filter description is clear. The sample-count caveat at lines 105-109 should be cross-referenced here because it controls whether the lightweight update stays cheap.
- Lines 41-47: the PGH adaptation is well explained. The factor 1.25 is empirical, so do not make it sound derived from optimality.
- Lines 57-63: exponential median-error decay is a numerical result, not a rigorous convergence theorem.
- Lines 65-69: the `10^7` comparison to Kitaev is parameter- and implementation-specific. It is useful, but should not be presented as a general asymptotic separation.
- Lines 71-89: the decoherence and unmodeled-noise sections are good. The note correctly describes graceful degradation rather than full robustness.
- Lines 93-101: the restart protocol is important because median performance hides tails. Keep this near the headline results.
- Lines 127-133: the limitations are excellent and should remain.

## Missing or Suggested Additions

- Add a one-sentence output guarantee distinction: RFPE gives a posterior estimate and credible interval under its Bayesian approximation, not the exact bit string output of textbook QPE.

