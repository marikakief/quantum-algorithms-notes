# Review: A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013)

Source note: [[A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013) — Paper Notes]]

## Primary Sources Checked

- Wallach, "A quantum polylog algorithm for non-normal maximal cyclic hidden subgroups in the affine group of a finite field", arXiv:1308.1415.

## Verdict

Minor issues. The note is a good concise summary. It should fix the success-parameter notation and add a title heading.

## Line-Anchored Comments

- Lines 1-5: missing top-level title.
- Lines 18-26: problem statement is clear. The promise that the hidden subgroup is one of the `C_b` is the whole scope of the result.
- Lines 28-32: good assessment; this is not a general affine-HSP solver.
- Lines 36-47: algorithmic structure is sound. The fixed-vector/wavelet view is the right explanatory lens.
- Lines 52-58: the complexity expression should use something like `polylog(q) * polylog(1/epsilon)`. As written, `log(epsilon)^2` is odd for `epsilon < 1`.
- Lines 63-65: excellent scope caveat.
- Lines 68-71: reusable ideas are well chosen.

## Missing or Suggested Additions

- Add a sentence saying how the algorithm identifies/samples the large irrep component with sufficient probability, because that is the bridge from a coset state to the wavelet vector.

