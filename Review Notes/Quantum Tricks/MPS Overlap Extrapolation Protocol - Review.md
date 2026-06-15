# Review: MPS Overlap Extrapolation Protocol

Source note: [[MPS Overlap Extrapolation Protocol]]

## Primary Sources Checked

- Berry et al., arXiv:2409.11748 / PRX Quantum 6, 020327 (2025).

## Verdict

Minor issues. The card is useful, but it should make the empirical nature of the extrapolation harder to miss.

## Line-Anchored Comments

- Lines 6-16: the overlap-extrapolation workflow is clear.
- Lines 22-31: if the card quotes overlap or squared-overlap values, label them consistently. This paper is easy to misread because both conventions appear in discussions.
- Lines 35-42: extrapolating finite-bond-dimension MPS overlap is a model-dependent empirical procedure. It is not a rigorous lower bound on QPE success probability unless the extrapolation uncertainty is controlled.
- Lines 50-58: the connection to filtering/search cost is good. Make the dependency on overlap explicit: amplitude overlap and squared overlap enter different formulas.

## Missing or Suggested Additions

- Add a "do not use as a theorem" warning. The protocol is valuable evidence for Fe-S/FeMoCo-like systems, not a general guarantee for strongly correlated chemistry.

