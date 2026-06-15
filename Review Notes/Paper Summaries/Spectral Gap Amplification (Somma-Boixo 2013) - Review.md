# Review: Spectral Gap Amplification (Somma-Boixo 2013)

Source note: [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]

## Primary Sources Checked

- Somma and Boixo, "Spectral Gap Amplification," SIAM Journal on Computing 42, 593-610 (2013), arXiv:1110.2494.

## Verdict

Minor issues. The note gives a good account of the walk construction and the oracle lower bounds, but it should be more careful about normalization, the ancilla-extended eigenstate, and the black-box scope of the impossibility theorem.

## Line-Anchored Comments

- Line 9: the amplified Hamiltonian does not literally keep the same state in the same Hilbert space; it keeps the same system eigenstate tensored with an ancilla state, up to the construction's invariant subspace.
- Lines 17, 57-59, and 83: the two gap numbers should be presented together to avoid confusion. The unscaled walk-like operator has gap `Omega(sqrt(Delta/L))`; after scaling by `L` and adding the penalty term, the gap is `Omega(sqrt(Delta L))` with norm `O(L)`. Algorithmic comparisons need the normalized/resource-aware gap.
- Line 21: "no spectral gap amplification is possible" is too absolute. The impossibility result is a black-box/oracle statement under the paper's assumptions about preserving the relevant ground state and not increasing simulation cost.
- Line 37: one black-box call to `O_H` is true in the oracle model for controlled projector reflections. It should not be read as a statement about compiling arbitrary local projector terms in a physical circuit.
- Line 63: the simulation-cost statement is important because it is where the amplification stops being a naive rescaling. It would help to say this is efficient in the black-box query model, with locality overhead handled separately.
- Lines 71-75: the stoquastic/QMC consequence is interesting but should be worded as an oracle lower-bound consequence for some families, not as a blanket claim about all stoquastic Monte Carlo mappings.
- Line 97: the adiabatic-circuit-simulation speedup claim needs a citation or more detail about the local construction and dimensional assumptions. As written, the exponent `1 + 1/d` is hard to verify from the surrounding text.
- Lines 103-106: the caveats are good. Define "AST" on line 106, or replace it with the full phrase.
- Lines 143-147: the later spectrum-amplification links are useful, but they should be clearly marked as later developments, not part of Somma-Boixo's 2013 result.

## Missing or Suggested Additions

- Add a short normalization paragraph: "The phrase quadratic amplification refers to improving the small eigenvalue scale from `Delta/L` to roughly `sqrt(Delta/L)` in the walk picture; after scaling back by `L`, the absolute gap becomes `sqrt(Delta L)`."

