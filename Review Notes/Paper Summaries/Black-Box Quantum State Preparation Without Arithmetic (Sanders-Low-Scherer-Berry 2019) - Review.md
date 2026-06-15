# Review: Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019)

Source note: [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]]

## Primary Sources Checked

- Sanders, Low, Scherer, and Berry, "Black-box quantum state preparation without arithmetic", arXiv:1807.03206 / Physical Review Letters 122, 020502 (2019).

## Verdict

Minor issues. The main trick is well captured. The review should correct one misleading word about "collapse" and keep the oracle-cost caveat visible.

## Line-Anchored Comments

- Lines 9-15: good problem/cost framing.
- Lines 21-23: the improvement factor is useful; specify it is for the precision and baseline arithmetic in the paper's comparison.
- Lines 41-53: the inequality-test construction is clear, but line 53 should not say the `ref` register "collapses." No measurement occurs there; unpreparing the uniform superposition creates the desired amplitude on the all-zero `ref` state by interference.
- Lines 57-64: the linear versus root coefficient distinction is useful.
- Lines 66-70: good caveat that Cartesian root coefficients still need arithmetic.
- Lines 76-82: "comparator costs exactly `n` Toffolis" should be tied to the comparator construction and cost model used by the paper.
- Lines 97-103: excellent caveats. Move the oracle-model caveat closer to the headline `n`-Toffoli claim.

## Missing or Suggested Additions

- Add one sentence explaining that amplitude amplification still depends on the norm/success amplitude. The comparator removes arcsine arithmetic, not the need to amplify low-success preparations.

