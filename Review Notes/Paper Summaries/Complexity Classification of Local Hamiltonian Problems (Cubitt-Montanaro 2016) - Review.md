# Review: Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016)

Source note: [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]

## Primary Sources Checked

- Cubitt and Montanaro, "Complexity classification of local Hamiltonian problems," SIAM Journal on Computing 45(2), 268-316 (2016) / arXiv:1311.3161.

## Verdict

Minor issues. The note is strong and captures the classification philosophy. It mainly needs sharper caveats around allowed coupling signs and the precise theorem settings.

## Line-Anchored Comments

- Lines 7-22: the `S`-Hamiltonian setup is clear. Add that the allowed scalar multiples/coupling signs are part of the problem definition; physical sign restrictions can change complexity.
- Lines 26-30: the headline classification is accurate for the paper's setting. "No intermediate behaviour" should be scoped to fixed 2-qubit interaction languages under the paper's assumptions.
- Lines 38-61: the Pauli-correlation normal form is a useful explanation. It would help to distinguish applying the same one-qubit basis change to both qubits from arbitrary independent local basis changes.
- Lines 67-75: the gadget and symmetry-breaking discussion is good, especially for Heisenberg. Add that these are perturbative reductions with large energy-scale separations, not direct physical equivalences.
- Lines 85-108: theorem summaries are helpful. For the StoqMA and NP branches, include one sentence explaining the difference between diagonal classical interactions and stoquastic but non-diagonal interactions.
- Line 95: the "equal-weight 2-body couplings" phrase should be checked. The result has restrictions to lattice/equal interaction forms in some cases, but the available weights and gadgets are subtle enough that this should not be summarized too casually.
- Lines 136-143: caveats are excellent and should stay near the theorem statements.

## Missing or Suggested Additions

- Add a small table of assumptions: free local terms or not, 2-qubit only or constant locality, arbitrary real couplings or sign-restricted, lattice-restricted or not.

