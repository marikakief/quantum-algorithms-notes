# Review: Oracle-to-Hamiltonian Simulation via Swap Conjugation

Source note: [[Oracle-to-Hamiltonian Simulation via Swap Conjugation]]

## Primary Sources Checked

- Childs et al., "Exponential algorithmic speedup by quantum walk", arXiv:quant-ph/0209131: https://arxiv.org/abs/quant-ph/0209131

## Verdict

Sound with minor caveats. The card captures the important construction: use an edge-coloured oracle to conjugate a simple swap-like Hamiltonian, producing the graph adjacency action on the clean subspace.

## Line-Anchored Comments

- Line 10: "can't be simulated directly as a sum of local terms"

  Right for arbitrary random vertex names. The point is not merely nonlocality but black-box access: the graph adjacency is known only through the neighbor oracle.

- Line 14: `T|a,b,1> = 0`

  This is a Hamiltonian term, not a unitary swap operation on the whole register space. The card should avoid calling `T` a swap operator without noting the flag-projector restriction.

- Lines 16-20: construction is right.

  The reason `V_c^\dagger T V_c` maps the clean subspace to neighbors is that edge colour `c` must be consistent at both endpoints. That consistency condition deserves explicit mention.

- Line 22: product-formula simulation is historically accurate.

  Modern Hamiltonian simulation would implement this differently, but for the 2003 proof the Lie-product-formula sandwich is the relevant tool.

- Lines 34-35: edge-colouring caveat is important.

  Keep it. It is one of the least obvious assumptions in the oracle implementation.

## Missing or Suggested Additions

- Add a one-line statement of the clean subspace invariant: the algorithm begins and ends in states `|a,0,0>`; auxiliary garbage registers are only transient inside the simulation term.
