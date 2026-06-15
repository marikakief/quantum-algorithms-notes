# Review: Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007)

Source note: [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes]]

## Primary Sources Checked

- Ben-Or and Hassidim, "Quantum search in an ordered list via adaptive learning", arXiv:quant-ph/0703231 / FOCS 2008 version under the Bayesian-learner title.

## Verdict

Major numerical issue. The conceptual link between noisy Bayesian search and ordered quantum search is good, but the quantitative explanation around the `0.32 log n` constant contains impossible information-theoretic wording.

## Line-Anchored Comments

- Add a top-level title.
- Lines 20-34: the high-level contributions are right. The paper's headline is "less than `(log_2 n)/3`"; if the note uses `<0.32 log_2 n`, verify the exact constant and version.
- Line 28: specify that logs in the binary-channel mutual information formula are base 2 if the query count is in `log_2 n` units.
- Lines 69-80: this block needs correction. It says a subroutine on `K=223` outcomes yields `18.5625` bits of information. That cannot literally be bits of Shannon information, since `log_2 223 < 8`. Either the parameter `K` is mistyped, the quantity `I` is not being measured in bits, or the note has imported a paper-specific normalization without explaining it.
- Line 80: `6/18.5625` is about `0.323`, not `<0.32`. This also conflicts with line 32.
- Lines 112-119: the lower-bound constant is useful, but label the error model carefully: this is ordered search, not unordered Grover search.
- Lines 130-136: good caveats. Add that the numerical constant depends on a specific optimized subroutine and should not be treated as a clean closed-form theorem without the paper's normalization.

## Missing or Suggested Additions

- Recompute or quote the ordered-search constant directly from the paper, with the same logarithm base and the same definition of the information quantity. As written, the note cannot be trusted for the numerical derivation.

