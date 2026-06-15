# Review: Shared Exponential Evaluation Across Pseudopotential Terms

Source note: [[Shared Exponential Evaluation Across Pseudopotential Terms]]

## Primary Sources Checked

- Berry et al., arXiv:2312.07654.

## Verdict

Minor issues. The card explains the engineering trick clearly. The main risk is phrasing the GTH-specific parameter matching as if it applied to all pseudopotentials.

## Line-Anchored Comments

- Lines 11-18: good setup of the shared Gaussian/exponential structure.
- Lines 21-24: the `q=0` trick for the local term is the key insight and is well stated.
- Line 26: the controlled-copy description is plausible; it should specify whether "zero" is loaded into the working momentum register or selected by a mux to avoid accidental overwrite/uncompute issues.
- Line 28: "the polynomial coefficients are the same" is too broad unless it is explicitly GTH/HGH-specific. Say the local terms can be embedded into the same indexed arithmetic pipeline for the parameterization considered in the paper.
- Line 38: the savings estimate is useful but should be tied to the chosen interpolation order and precision. For some parameter regimes the avoided exponential is not the only dominant cost.
- Lines 42-44: caveats are good. Keep the warning that PAW/other pseudopotential families may not share the same radial structure.

## Missing or Suggested Additions

- Add a short note that the trick reduces block-encoding cost but does not by itself reduce the large `lambda_nonloc` normalization.

