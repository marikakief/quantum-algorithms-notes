# Review: A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014)

Source note: [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]

## Primary Sources Checked

- Farhi, Goldstone, and Gutmann, "A Quantum Approximate Optimization Algorithm," arXiv:1411.4028 (2014).

## Verdict

Minor-to-major issues. The core QAOA summary is good and the skepticism is appropriate, but the note mixes a paper summary with a fast-moving literature survey. Some resource statements also need correction.

## Line-Anchored Comments

- Lines 19-25: the main summary is good. "Approximation quality improves with `p`" should be stated as the optimized value `M_p` is nondecreasing; actual found values depend on optimizer performance.
- Lines 53-55: "Total depth `O(p m)`" is really a gate-count or serial-depth upper bound. For bounded-degree/local graphs, commuting terms and edge coloring can give much smaller parallel depth.
- Lines 59-63: fixed-`p` preprocessing is overstated. Local contribution values are independent of `n` for bounded degree and fixed `p`, but counting all local neighborhoods in an input graph still costs at least linear time in the graph size.
- Lines 67-81: the locality/concentration argument is the strongest technical part of the original-paper summary. Keep it, with the preprocessing correction above.
- Lines 85-93: key results are accurate. The convergence as `p -> infinity` should mention that the required `p` may scale with the adiabatic runtime and is not a practical finite-depth guarantee.
- Lines 97-176: the later-literature survey is useful but too broad for a review of the 2014 paper. Mark it as a maintenance-prone survey, or split it into a separate QAOA landscape note.
- Lines 167-176: the "as of early 2026" section needs periodic verification. This is exactly the kind of statement that will age quickly.
- Lines 208-216: the caveats are strong and should stay. Add optimizer cost and parameter concentration/transfer as separate caveats rather than treating optimization only under barren plateaus.

## Missing or Suggested Additions

- Add a clear boundary between "what the 2014 paper proves" and "subsequent status of QAOA." The current note is valuable, but a reader could confuse later classical catch-up and sampling-hardness results with claims in the original paper.

