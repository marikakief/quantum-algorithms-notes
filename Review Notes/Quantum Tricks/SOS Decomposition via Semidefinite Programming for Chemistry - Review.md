# Review: SOS Decomposition via Semidefinite Programming for Chemistry

Source note: [[SOS Decomposition via Semidefinite Programming for Chemistry]]

## Primary Sources Checked

- Low et al., arXiv:2502.15882 / Physical Review X 15, 041016 (2025).

## Verdict

Minor issues. The card is conceptually good, but it slightly overstates how automatic the SOS construction is.

## Line-Anchored Comments

- Line 10: an electronic-structure Hamiltonian can be shifted/factored abstractly, but an efficient chemistry-structured SOS decomposition is a nontrivial optimization result. State that distinction.
- Lines 17-24: the SDP/lower-bound framing is useful. The reader should know whether the SDP is exact, approximate, or solved within a chosen ansatz family.
- Line 27: "dual SDP gives SOS for free" is too strong. The dual solution can certify or suggest the SOS structure, but one still pays classical optimization and numerical-conditioning costs.
- Lines 35-42: if rank reductions are quoted, mark them as empirical and molecule-dependent.

## Missing or Suggested Additions

- Add a warning about numerical precision and certification. For a lower bound used inside spectrum amplification, small classical errors can matter algorithmically.

