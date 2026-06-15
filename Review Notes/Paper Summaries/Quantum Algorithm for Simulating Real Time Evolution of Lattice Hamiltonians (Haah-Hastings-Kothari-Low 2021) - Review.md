# Review: Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021)

Source note: [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]]

## Primary Sources Checked

- Haah, Hastings, Kothari, Low, "Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians," SIAM Journal on Computing 52(6), FOCS18-250-FOCS18-284 (2023; FOCS 2018) / arXiv:1801.03922.

## Verdict

Minor issues. The note gives a good account of the Lieb-Robinson decomposition and the gate-complexity lower bound. A few quantitative and bibliographic details need cleanup.

## Line-Anchored Comments

- Lines 13-18: the sparse-simulation comparison is directionally right, but the `O(n^2 T)` gate estimate depends on a particular gate implementation of sparse oracles. State this as a gate-level comparison, not as the query complexity of QSP/Taylor itself.
- Lines 40-52: the decomposition identity is the right central object. Add that the plus sign on the overlap evolution is an inverse/backward evolution compensating for double counting.
- Lines 65-74: good full-algorithm summary. The block-simulation cost should specify that each block is polylogarithmic in global `nT/epsilon` because block size is `polylog(nT/epsilon)`.
- Lines 80-86: "reduces the total number of block simulations from `m` to `3m/2` or `2m/3` depending on the counting" is internally inconsistent. Re-check the merge optimization and replace with the paper's exact count.
- Lines 100-108: the lower-bound summary is useful. Mention that it is for time-dependent/piecewise-constant Hamiltonians; the time-independent lower-bound question is separated later.
- Line 160: the link target says Low-Wiebe but the displayed text says Kieferova-Scherer-Berry. Fix the reference label.
- Lines 132-138: the caveats are strong and should stay.

## Missing or Suggested Additions

- Add a one-line comparison to Childs-Su product-formula lattice simulation: both achieve near-linear spacetime scaling, but HHKL gets polylogarithmic precision dependence through small-block simulation rather than high-order global product formulas.
