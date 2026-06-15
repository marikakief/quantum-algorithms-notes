# Review: QROM Interpolation for Negative Exponential

Source note: [[QROM Interpolation for Negative Exponential]]

## Primary Sources Checked

- Berry et al., arXiv:2312.07654.
- Sanders, Berry, Babbush et al., arXiv:1902.10673 for QROM interpolation/function-evaluation background.

## Verdict

Minor issues. The technique is captured well. The main problems are ambiguous constants and one inconsistent numerical-cost sentence.

## Line-Anchored Comments

- Lines 7 and 27: the cube-root QROM scaling for quadratic interpolation is a good takeaway. Phrase it as QROM table-size scaling for the interpolation grid, not total function-evaluation scaling.
- Lines 13-21: the base-2 conversion plus fractional interpolation plus integer shift explanation is clear and useful.
- Lines 23-25: the linear/quadratic arithmetic constants should be source-checked if used in downstream cost estimates. They are too specific to leave uncited.
- Lines 38-43: the table says totals of roughly 556 and 1,228 for `b=20`, then line 43 says total `~5,000` Toffolis for `b=20`, `delta_f=10^-6`. These may refer to different surrounding pseudopotential subroutines, but the card currently reads as inconsistent.
- Line 47: good caveat about the extra base-conversion multiplication.
- Line 49: "smooth and bounded" is not enough by itself; interpolation efficiency also depends on the chosen interval and derivative bounds. Add "on the specified finite interval with controlled derivatives."

## Missing or Suggested Additions

- Add the range of `z` or how the interval is normalized in the 2024 paper. Function-evaluation costs are not meaningful without the interval.

