# Review: Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014)

Source note: [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]]

## Primary Sources Checked

- Childs, Jao, and Soukharev, "Constructing elliptic curve isogenies in quantum subexponential time", arXiv:1012.4019 / Journal of Mathematical Cryptology 8 (2014).

## Verdict

Minor issues. This is a strong summary. The reduction to injective hidden shift and the GRH-based star-operator evaluation are both captured. The review should keep the ordinary/supersingular distinction and the space model visible whenever cryptographic relevance is discussed.

## Line-Anchored Comments

- Lines 11-31: good problem statement. The output is an ideal class specifying a horizontal isogeny; it is not merely a path found by random walks.
- Lines 41-45: excellent assessment. The oracle-evaluation cost is the part many short summaries omit.
- Lines 53-91: the factor-base and relation-finding story is accurate. Add that the split-prime ideals must avoid primes dividing `qn`, as later noted in the theorem statement.
- Lines 94-133: the hidden-shift reduction is clear. The action being free and transitive is essential; if the action were not injective, Kuperberg's hidden-shift machinery would not apply directly.
- Lines 139-189: the `L(1/2,c)` constants are correctly tied to star-operator evaluation under GRH.
- Lines 204-224: good separation between Kuperberg and polynomial-space Regev-style back ends. Keep the `sqrt(2)` penalty visibly attached to the hidden-shift solver, not to isogeny evaluation.
- Lines 239-245: the comparison table is useful. The sentence about ordinary rather than supersingular is essential and should not be buried.
- Lines 251-261: the caveats are all accurate. The algorithm's output as ideal-class description versus normalized map is an important distinction.

## Missing or Suggested Additions

- Add a one-line warning that "GRH only" means "only GRH beyond the usual computational model", not unconditional.
- When cross-linking to later isogeny cryptography, avoid implying this attacks SIDH/SIKE-style supersingular path problems; the note already says this correctly and should preserve that boundary.

