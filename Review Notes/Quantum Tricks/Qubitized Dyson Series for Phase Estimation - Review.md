# Review: Qubitized Dyson Series for Phase Estimation

Source note: [[Qubitized Dyson Series for Phase Estimation]]

## Primary Sources Checked

- Su et al., arXiv:2105.12767, interaction-picture / qubitized-Dyson sections.

## Verdict

Minor issues. The mechanism is well explained. The constant-factor comparison should be tied more tightly to the source assumptions and shifted-energy regime.

## Line-Anchored Comments

- Line 7: "factor of `3/2`" is a useful headline but not universal. It is the comparison for the source's Dyson-series phase-estimation setup, not for arbitrary time-dependent simulation tasks.
- Lines 11-15: good description of why qubitizing `sin(H tau)` can avoid deterministic time-evolution amplitude amplification.
- Line 15: the walk eigenphase expression should be handled carefully: there are sign branches and the phase-estimation inversion is valid only in the chosen energy window.
- Lines 17-20: the `tau=1/lambda_B`, `beta=e`, and step-count formula are source-specific and should mention the energy shift `E0`.
- Lines 23-29: the cost table is useful. It should identify what `C` includes, because "one Dyson-series LCU once" hides sorting, time-register preparation, and potential-oracle costs.
- Lines 35 and 41-47: the near-zero / energy-offset caveat is correct and important. Consider moving it before the cost comparison.
- Line 45: correct: this is for eigenvalue estimation, not a replacement for actual time evolution.

## Missing or Suggested Additions

- Add one warning that the arcsin nonlinearity is not a small correction if the target energy is not shifted into the linear regime.

