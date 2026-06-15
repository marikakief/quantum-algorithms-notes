# Review: The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa-An-Babbush-Berry 2023)

Source note: [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]]

## Primary Sources Checked

- Costa, An, Babbush, and Berry, "The discrete adiabatic quantum linear system solver has lower constant factors than the randomized adiabatic solver", arXiv:2312.07690 / Quantum 2025.

## Verdict

Minor issues. The numerical-validation role is well explained. The main correction is rhetorical: numerical evidence on small random matrices does not mathematically settle every possible structured instance, so the "it can't" phrasing about randomized adiabatic methods is too strong.

## Line-Anchored Comments

- Add a top-level title.
- Lines 7-13: good summary of the paper's purpose and headline constants.
- Line 13: "It also settles ... (It can't.)" overstates what a numerical benchmark paper can prove. Better: "On the benchmarked random instances, the randomized method is not competitive; this undermines the earlier practical-cost claim."
- Lines 21-39: the methodology is clear and appropriately concrete.
- Lines 43-48: good distinction between randomized evolution time and discrete walk steps. Keep this because it affects resource interpretation.
- Lines 52-60: the filter-cost partition is useful and should remain.
- Lines 66-75: the comparison table is good, but "tight" should mean empirically tight on tested instances, not a theorem about all matrices.
- Lines 79-89: excellent caveats about small matrix sizes, random instances, and missing T-count/resource estimates.

## Missing or Suggested Additions

- Add a short note on what was held fixed in the benchmark: target intermediate error, matrix distribution, and `p` schedule choice. Otherwise readers may overgeneralize the constants.

