# Review: Quantum Verification of Matrix Products (Buhrman-Špalek 2005)

Source note: [[Quantum Verification of Matrix Products (Buhrman-Špalek 2005) — Paper Notes]]

## Primary Sources Checked

- Buhrman and Špalek, "Quantum Verification of Matrix Products," SODA 2006, arXiv:quant-ph/0409035.

## Verdict

Minor issues. The note is detailed and mostly careful. The main checks are notation in the maintained compressed data and the graph-product terminology for the Johnson walk.

## Line-Anchored Comments

- Lines 11-19: good problem statement. Keep "integral domain" prominent; the Boolean semiring caveat later is not optional.
- Lines 25-27: excellent assessment. The note correctly sees that query speedup alone is not enough; the data-maintenance trick is central.
- Lines 33-34: defining wrong entries `W` early is useful.
- Lines 54-77: the `Verify Once` description is strong. The notation in line 63 is compressed; if this becomes a trick card, spell out which submatrix/vector dimensions are being multiplied.
- Lines 66-76: check whether the walk graph should be called a categorical/tensor product or simply the product of two Johnson graphs. The update "exchange one row and one column" is the important operational fact.
- Lines 83-107: the `k=n^{2/3}` explanation is clear and probably the best part of the note.
- Lines 117-143: good inclusion of `q(W)`. This prevents the expected-time claim from being oversimplified to a function only of `|W|`.
- Lines 155-163: lower-bound caveat is important. Do not present `Omega(n^{3/2})` as a lower bound for the full verification problem.
- Lines 165-188: good coverage of sparse-output and Boolean-product variants.
- Lines 209-213: caveats are strong and should remain.

## Missing or Suggested Additions

- Add a small dimension table for `p`, `q`, `R`, `S`, `a_R`, `b_S`, and `c_{R,S}`. It would prevent misreadings of the bilinear compression.

