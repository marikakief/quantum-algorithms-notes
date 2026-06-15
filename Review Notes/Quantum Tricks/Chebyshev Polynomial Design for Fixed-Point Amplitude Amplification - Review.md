# Review: Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification

Source note: [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]]

## Primary Sources Checked

- Yoder-Low-Chuang fixed-point search: https://arxiv.org/abs/1409.3305

## Verdict

Minor issues. The card is useful, but one formula should be checked against the paper before reuse.

## Line-Anchored Comments

- Lines 8 and 39: query complexity

  Correct at the scaling level: fixed-point behavior with `O(log(1/delta)/sqrt(lambda_0))` queries for guaranteed overlap `lambda_0`.

- Lines 18-25: phase and success-probability formulas

  High risk for transcription errors. The success-probability formula should be copied exactly from the paper, with `gamma` and reciprocal notation made explicit. The current denominator-style expression is ambiguous.

- Line 28: "can never overcook"

  Good intuition, but qualify: no overshoot for overlaps in the guaranteed range `lambda >= w`; below that range the polynomial can oscillate.

- Line 34: "minimum finding, collision problems"

  These applications need care. Fixed-point AA is a drop-in replacement only when the problem supplies a lower-bound promise on the relevant success probability or a coherent adaptive scheme.

## Missing or Suggested Additions

- Add the fixed-point property as a range guarantee: for all `lambda >= w`, failure probability is at most `delta^2`.
