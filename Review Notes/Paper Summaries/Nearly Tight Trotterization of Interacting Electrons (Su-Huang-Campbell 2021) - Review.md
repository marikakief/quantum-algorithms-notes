# Review: Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021)

Source note: [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]]

## Primary Sources Checked

- Su, Huang, Campbell, "Nearly tight Trotterization of interacting electrons," Quantum 5, 495 (2021) / arXiv:2012.09194.

## Verdict

Minor issues. The note captures the paper's main contribution: replacing a blunt spectral-norm Trotter analysis by a fermionic-seminorm analysis on the fixed-particle-number sector. The main repairs are scope and notation.

## Line-Anchored Comments

- Lines 24-30: the summary is directionally right, but the displayed gate complexity should state the assumptions on time, target precision, and the chosen product-formula order. As written it reads like a complete universal gate bound.
- Line 44: the equivalence to a projected norm is good. Make clear whether the projection is on both sides or encoded through the maximization over `eta`-electron bra/ket states.
- Line 50: the displayed nested commutator has a brace typo in `H_{\gamma_1}}`.
- Lines 71-77: "No dependence on `n`" is too strong if the matrix norms `||tau||`, `||tau||max`, and `||nu||max` themselves scale with basis size. The point is that no additional combinatorial `n` appears beyond the Hamiltonian norm parameters.
- Lines 83-90: the plane-wave comparison table is useful, but readers need the cost model: second quantization, asymptotic gate counts, and suppressed polylog/precision/time factors.
- Lines 112-116: the caveats are good. Add that concrete resource estimates still require evaluating Hamiltonian-specific coefficients, not only asymptotic exponents.

## Missing or Suggested Additions

- Add one sentence explaining why the fermionic seminorm can be much smaller than the full spectral norm: many creation/annihilation paths leave the fixed-`eta` sector or have zero matrix elements.
