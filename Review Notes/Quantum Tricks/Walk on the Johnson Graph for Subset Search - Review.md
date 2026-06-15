# Review: Walk on the Johnson Graph for Subset Search

Source note: [[Walk on the Johnson Graph for Subset Search]]

## Primary Sources Checked

- Ambainis, "Quantum walk algorithm for element distinctness", arXiv: https://arxiv.org/abs/quant-ph/0311001
- Szegedy, "Spectra of Quantized Walks and a sqrt(delta epsilon) rule", arXiv: https://arxiv.org/abs/quant-ph/0401053

## Verdict

Major issues. The element-distinctness cost derivation is right, but the generic walk-search line omits the spectral-gap factor and will mislead reuse.

## Line-Anchored Comments

- Lines 7-15: construction

  Good. The point of the Johnson walk is exactly to store an `r`-subset and update it by swapping one item, so setup costs `r` but movement costs only constant queries.

- Line 14: update cost

  In a reversible implementation there may be a query and an unquery; Ambainis counts constants away. Say "constant queries" rather than exactly 1 if this card is used outside asymptotic query counting.

- Line 16: generic step count

  This is the main problem. Walk search is not just `sqrt(number of vertices / marked vertices)`. The Johnson graph's spectral gap contributes a `sqrt(r)` factor. In MNRS notation, cost is governed by `1/sqrt(delta epsilon)`, with `delta = Theta(1/r)` for `J(N,r)`.

- Lines 22-26: ED derivation

  Correct because it does include the missing `sqrt(r)` factor: marked fraction about `r^2/N^2`, walk cost `sqrt(1/epsilon) * sqrt(1/delta) = (N/r) * sqrt(r) = N/sqrt(r)`, plus setup `r`.

- Lines 30-36: general `k`

  Correct for Ambainis's `k`-distinctness generalization, but avoid implying all small-certificate subset problems get this bound. Checking and update costs can dominate.

## Missing or Suggested Additions

- Add the MNRS cost template `S + (1/sqrt(epsilon))((1/sqrt(delta))U + C)` and then specialize it to the Johnson graph.
