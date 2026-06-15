# Review: Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Saenz-de-Cabezon 2013)

Source note: [[Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Sáenz-de-Cabezón 2013) — Paper Notes]]

## Verdict

Minor issues. This is a strong summary of a classical complexity paper, and it does a good job separating exact Euler-characteristic hardness from the normalized quantities used in quantum TDA. The main improvements are wording around Betti-number implications and avoiding a too-broad "quantum does not help with #P" slogan.

## Comments

- The statement that computing Euler characteristic is "at most as hard as computing all Betti numbers" is correct in the sense that the alternating sum of all Betti numbers gives the invariant. It should not be read conversely: #P-hardness of exact Euler characteristic does not by itself prove #P-hardness of every individual Betti number under every input model.

- The note correctly emphasizes the facet-input model. Keep that caveat next to the #P-completeness claim, because later TDA work usually uses clique-complex input from a graph or distance oracle.

- The comparison table's line "quantum doesn't help with #P" is too coarse. A safer sentence is: "There is no known generic polynomial-time quantum algorithm for exact #P-hard Euler-characteristic computation; the quantum TDA algorithms target normalized additive-approximation promise problems instead."

- The reduction sketch is clear. It would help to state explicitly that the parity-sum cancellation is the reason a counting problem turns into an Euler-characteristic value, rather than merely an inclusion-exclusion coincidence.

- The note is right to call this a classical baseline. It should not imply that Euler characteristic is a weaker/easier invariant for all representations; the hardness is representation-dependent.

## Suggested Additions

- Add a one-line "input models" box comparing facet input, clique-complex input, and oracle/simplex-membership input.
- Add a short caution that reduced Euler characteristic has a sign/convention shift from the unreduced Euler characteristic.
