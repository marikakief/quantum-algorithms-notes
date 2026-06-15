# Review: Standard Amplitude Amplification

Source note: [[Standard Amplitude Amplification]]

## Primary Sources Checked

- BHMT, "Quantum Amplitude Amplification and Estimation": https://arxiv.org/abs/quant-ph/0005055
- Grover original search: https://arxiv.org/abs/quant-ph/9605043
- BBHT tight search: https://arxiv.org/abs/quant-ph/9605034

## Verdict

Minor issues. The card is concise and mostly correct.

## Line-Anchored Comments

- Lines 7 and 25: "near 1" and `>= 1-a`

  Correct for standard AA with the usual iteration count. Avoid saying "certainty" unless also discussing exact amplitude amplification with adjusted phases.

- Lines 12-15: operator/reflection definitions

  Good. It may help to mention sign conventions vary and do not affect the measured success probability.

- Line 36: "Optimal - matches lower bound"

  Correct in the unstructured-search black-box setting. For a general algorithm `A`, the lower-bound justification is by reduction to search; make clear the optimality claim is black-box/query-model optimality.

- Line 40: "measurement-free"

  Good. Add that classical randomness in `A` must be coherently represented as a seed register if one wants to amplify a randomized heuristic.

## Missing or Suggested Additions

- Add one line on unknown `a`: BBHT randomized schedule finds a good item; amplitude estimation estimates `a`; these are different goals.
