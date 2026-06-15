# Review: Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012)

Source note: [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]]

## Primary Sources Checked

- Montanaro, "The quantum query complexity of learning multilinear polynomials", Information Processing Letters 2012 / arXiv:1105.3310.

## Verdict

Minor issues. This is a clean and accurate query-complexity note. The main improvements would be more explicit oracle-model and degree-growth caveats.

## Line-Anchored Comments

- Add a top-level title.
- Lines 9-25: good problem setup and correct identification of the Reed-Muller case over `F_2`.
- Lines 27-37: the `Theta(n^{d-1})` versus `Omega(n^d)` contrast is right for constant degree. Keep the "constant degree" qualifier attached to every asymptotic summary.
- Lines 59-75: good Holevo-Fano lower-bound explanation. It may be worth stating that the lower bound is for learning/identifying the whole polynomial, not for evaluating it at a point.
- Lines 83-112: the finite-difference linearization is the right central trick.
- Lines 126-136: the exact query count is useful. If `d` grows, the binomial sum and `2^{d-1}` derivative cost dominate; the caveat at lines 165-166 is important.
- Lines 138-151: good finite-field Bernstein-Vazirani summary. Add one sentence about trace characters only being needed for non-prime fields.
- Lines 168-171: strong caveats on arithmetic and finite-field QFT cost.

## Missing or Suggested Additions

- Add a small worked example for `d=2`, where one derivative turns a quadratic into a linear function. This would make the method much easier to remember.

