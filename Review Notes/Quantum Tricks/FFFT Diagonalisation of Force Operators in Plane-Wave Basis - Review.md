# Review: FFFT Diagonalisation of Force Operators in Plane-Wave Basis

Source note: [[FFFT Diagonalisation of Force Operators in Plane-Wave Basis]]

## Primary Sources Checked

- O'Brien et al., arXiv:2111.12437 / Physical Review Research 4, 043210 (2022).
- Su et al., arXiv:2105.12767 / PRX Quantum 2, 040332 (2021), for first-quantized plane-wave context.

## Verdict

Minor issues. The card is broadly correct, but it should be more explicit about which force terms share the diagonal basis and what normalization is being compared.

## Line-Anchored Comments

- Line 7: "all force operators on all nuclei commute" should be read as a statement about the relevant diagonal potential-force operators in the position/plane-wave-dual basis. If kinetic/basis-response terms are included, this wording becomes too broad.
- Line 21: saying `lambda_F` is the same as `lambda_H` is safe only asymptotically or for the specific plane-wave force construction. The Hamiltonian normalization includes several physical terms, so exact equality is too strong.
- Lines 29-35: the FFFT reuse story is good. Mention whether the FFFT cost is paid once per force component, per measurement setting, or amortized across components.

## Missing or Suggested Additions

- Add a one-sentence distinction between "diagonal in the same basis" and "simultaneously measurable with no further precision/normalization cost." The former does not automatically imply the latter.

