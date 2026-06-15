# Review: Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011)

Source note: [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/1102.1360
- PRL DOI linked in the source note.

## Verdict

Minor issues. The note correctly identifies the derivative-free time-dependent Trotter result and the Shannon-style counting argument. Tighten the implementation model for the time-ordered one-term evolutions and avoid making the randomized ordinary-exponential conversion sound deterministic.

## Line-Anchored Comments

- Lines 15-21: Good summary. The central distinction is exactly that the simulation theorem does not require derivative bounds on `H(t)`, while the counting argument uses the simulation theorem to bound the number of reachable states.

- Lines 27-36: The two-term generalized Trotter formula is described well. The note should stress that the factors are themselves time-ordered exponentials of individual local terms, not ordinary gates unless those terms commute with themselves at different times or are separately simulable.

- Lines 37-49: The gate-count derivation is useful but simplified. If it remains in the note, label it as the simplified big-O path rather than the exact theorem statement.

- Lines 57-67: The randomized conversion to ordinary product formulas is important, but "standard Trotter on the samples" should say the guarantee is probabilistic/in expectation and adds sampling overhead. It is not the same as a deterministic product formula for an arbitrary rapidly varying operator-valued term.

- Lines 71-75: The theorem/corollary bullets are accurate at the conceptual level. Add the bounded local-norm and efficient local-term simulation assumptions.

- Lines 89-99: The limitations section is strong. Line 99 should be promoted: if a rapidly changing Hamiltonian is only defined by an expensive classical function, the BQP simulation statement does not include that external description cost.

## Missing or Suggested Additions

- Add the title's "illusion of Hilbert space" point explicitly: although Hilbert space is exponentially large, polynomial-time local dynamics only reaches a tiny, circuit-countable subset.

- Add a comparison to Wiebe-Berry-Hoyer-Sanders: derivative-free but worse asymptotic precision/order versus smooth high-order formulas that need derivative control.

