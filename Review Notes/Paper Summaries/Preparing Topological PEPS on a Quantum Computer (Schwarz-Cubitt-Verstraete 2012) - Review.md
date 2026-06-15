# Review: Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012)

Source note: [[Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012) — Paper Notes]]

## Verdict

Minor issues. This is a careful and useful summary. The important promises are already present; they should be moved even closer to the main runtime claim.

## Comments

- The publication metadata in the note says Physical Review A 88, 032321 (2013), while the filename says 2012. That is fine if the filename uses the arXiv year, but the note should make the convention explicit.

- The runtime should be read as conditional on a polynomial lower bound for the spectral gaps of all intermediate parent Hamiltonians. The note states this later; repeat it immediately after the runtime formula.

- The algorithm prepares a state in the degenerate ground space, not a specified topological sector unless extra boundary/sector information is supplied.

- The QPE measurement of parent-Hamiltonian projectors hides Hamiltonian-simulation cost and locality assumptions. The note mentions this, but it should be attached to the algorithm table.

- The G-injective condition-number restriction to the symmetric subspace is the right technical highlight. Clarify that the full tensor map can be singular outside that subspace.

## Suggested Additions

- Add a one-line distinction between injective, G-injective, and G-isometric PEPS.
- Add an explicit "not a gap theorem" caveat: the paper assumes rather than proves efficient gaps for broad PEPS families.
