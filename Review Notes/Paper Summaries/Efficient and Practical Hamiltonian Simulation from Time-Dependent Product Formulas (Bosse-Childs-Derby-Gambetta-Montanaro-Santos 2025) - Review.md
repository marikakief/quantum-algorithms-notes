# Review: Efficient and Practical Hamiltonian Simulation from Time-Dependent Product Formulas (Bosse-Childs-Derby-Gambetta-Montanaro-Santos 2025)

Source note: [[Efficient and Practical Hamiltonian Simulation from Time-Dependent Product Formulas (Bosse-Childs-Derby-Gambetta-Montanaro-Santos 2025) — Paper Notes]]

## Primary Sources Checked

- Bosse, Childs, Derby, Gambetta, Montanaro, Santos, "Efficient and practical Hamiltonian simulation from time-dependent product formulas," arXiv:2403.08729: https://arxiv.org/abs/2403.08729

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 15-23: the THRIFT/Magnus-THRIFT/Fer-THRIFT structure is clearly explained. The `alpha^2` optimality caveat is properly scoped later, but should be mentioned in the headline sentence too.
- Lines 36-42: the first-order THRIFT formula is correct in spirit. Add that it uses time-ordered evolutions of individual interaction-picture terms, which are implementable because they are conjugated by the easy `H0`.
- Lines 65-69: the Fer expansion scaling statement is dense. Readers need to know when exact implementation of the Fer exponentials is realistic versus merely formal.
- Lines 82-84: numerical headline claims are useful, but keep model sizes and assumptions visible; otherwise "5x fewer gates" may overgeneralize.
- Lines 101-104: the limitations section is strong and should stay.

## Missing or Suggested Additions

- Add a direct comparison to corrected product formulas in terms of available primitives: THRIFT needs evolutions under `H0 + alpha H1^gamma`; CPF needs exponentials of `A`, `B`, and compiled commutator correctors.
