# Review: Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022)

Source note: [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]]

## Primary Sources Checked

- Boulebnane and Montanaro, "Solving boolean satisfiability problems with the quantum approximate optimization algorithm," arXiv:2208.06909 (2022).

## Verdict

Minor issues. The note is careful and unusually good about distinguishing average success probability, median runtime, and empirical advantage. The remaining issues are mostly about keeping the theorem's parameter regime visible whenever the performance numbers are quoted.

## Line-Anchored Comments

- Lines 9-25: good statement of the random formula model. Mention that clauses are sampled with replacement, so this is the paper's Poissonized ensemble rather than the most common fixed-clause SAT benchmark.
- Lines 53-60: correct objective. Add that `1/p_succ` is a sample count, not full runtime, unless the cost of preparing the QAOA state and checking satisfaction is included.
- Lines 68-78: this is the right high-level summary. Keep the caveat that the theorem is average-case, fixed-depth, fixed-angle, and small-`gamma`.
- Lines 123-140: the type-count expansion is a good transferable explanation. It should note that the number of types grows as `2^(2p+1)`, so this analytic method is fixed-`p`; it is not an efficient large-`p` classical calculation without further structure.
- Lines 142-172: the saddle-point theorem should be presented together with the assumptions: `k=2^q`, sufficiently small `gamma`, fixed `p`, and large `n`.
- Lines 198-205: the numerical comparison is well caveated. Keep the distinction between median-runtime fits and average-success-probability extrapolation whenever citing the `p=60` numbers.
- Lines 207-215: amplitude amplification is correctly separated from near-term QAOA. Add that coherent satisfiability checking and reflection about the prepared QAOA state are part of the cost.
- Lines 231-237: the caveat section is strong and should remain near the top if the note is shortened.

## Missing or Suggested Additions

- Add a one-line "not a quantum advantage theorem" label before the SAT-solver comparison table. The note already says this, but the label would prevent the performance numbers from being quoted out of context.

