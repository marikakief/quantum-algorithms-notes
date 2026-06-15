# Review: Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004)

Source note: [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]

## Primary Sources Checked

- Aharonov, van Dam, Kempe, Landau, Lloyd, and Regev, "Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation," FOCS 2004 / SIAM Journal on Computing 37(1), 166-194 (2007) / quant-ph/0405098.

## Verdict

Minor issues. The note is detailed and technically valuable. The main correction is wording: the paper constructs local Hamiltonians for adiabatic computation; calling this an "explicit sparse Hamiltonian simulation" can be confused with a Hamiltonian-simulation algorithm.

## Line-Anchored Comments

- Lines 7-20: excellent overview. Replace "explicit sparse Hamiltonian simulation" with "explicit sparse/local adiabatic Hamiltonian construction" to avoid confusion with digital Hamiltonian simulation.
- Lines 33-45: the adiabatic-theorem statement is fine as a paper-specific sufficient condition. Add that modern adiabatic theorems have different smoothness/gap dependencies; this is the theorem used for the equivalence proof.
- Lines 49-76: the history-state explanation is strong. Mention that padding is needed because the unpadded history state has only `1/(L+1)` probability on the final time.
- Lines 78-123: the 5-local construction is clear. The note correctly explains why the propagation terms are local checks rather than requiring the final output state.
- Lines 125-204: the spectral-gap analysis is the strongest part of the note. Keep the Markov-chain mapping and monotonicity bootstrap; those are often omitted in summaries.
- Lines 206-240: good explanation of the full-gap lift from the invariant subspace. Add that the final full gap is worse than the restricted path gap because of input-penalty/geometric-lemma effects.
- Lines 254-269: the 3-local reduction is well described. Define `K` and the relation between `J`, `epsilon`, and `L` wherever this is reused.
- Lines 275-316: the nearest-neighbor construction uses higher-dimensional particles/states in the grid construction, not ordinary 2-local qubit Hamiltonians. Make that explicit near the theorem headline.

## Missing or Suggested Additions

- Add a compact resource summary: locality, local dimension, minimum gap, adiabatic time, and padding overhead for the 5-local, 3-local, and 2-local-nearest-neighbor constructions.

