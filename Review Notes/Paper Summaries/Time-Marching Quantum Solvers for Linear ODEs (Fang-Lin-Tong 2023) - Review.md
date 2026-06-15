# Review: Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023)

Source note: [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]

## Primary Sources Checked

- Fang, Lin, and Tong, "Time-marching based quantum solvers for time-dependent linear differential equations", Quantum 2023 / arXiv:2208.06941.

## Verdict

Major issues. The structural summary is strong, especially the amplification-ratio viewpoint, but one complexity simplification is inconsistent with the displayed theorem and several comparisons to QLSA-based methods are too broad.

## Line-Anchored Comments

- Lines 15-19: excellent definition of `Q`. This is the right hardness parameter for the paper.
- Lines 25-32: good high-level explanation of why USVA plus compression fixes the naive time-marching success-probability collapse.
- Lines 46-52: "approximates `x/||x||`" is not quite the right way to describe uniform singular value amplification. The polynomial rescales singular values over a promised interval so the block-encoding normalization matches the operator norm; it is not a vector normalization map.
- Lines 73-77: the theorem statement is useful and appears consistent with the paper's main Dyson-series result.
- Lines 81-85: the lower-bound intuition is fine, but the constructed ODE/search reduction should be stated more cautiously. The phrase "set `A=-U_targ`" is too informal for a valid ODE instance and hides the promise construction.
- Lines 101-109: the QLSA comparison table should distinguish older diagonalisation-based ODE algorithms from later log-norm/history-state algorithms. Not all QLSA-based ODE solvers require diagonalizability in the same way.
- Line 115: spectral methods do not simply require "higher smoothness" in the abstract; the relevant issue is quantitative derivative bounds that enter the convergence and gate-complexity analysis.
- Line 126: the high-precision-limit scaling stated here does not follow from the displayed formula in lines 91-95. If the first term in the max dominates, line 93 gives roughly `alpha * alpha_comm * T^3 * Q / epsilon` times logs, not `alpha_comm^2 T^4 Q^3 / epsilon^2`. Recompute this sentence from Theorem 14.

## Missing or Suggested Additions

- Separate matrix-query cost from initial-state-query cost throughout; this is one of the paper's central selling points.
- Add a small warning that `Q` is partition-dependent in the algorithmic analysis and can overestimate difficulty for some non-normal dynamics.
