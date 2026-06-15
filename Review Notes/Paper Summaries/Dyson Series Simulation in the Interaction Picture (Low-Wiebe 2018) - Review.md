# Review: Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018)

Source note: [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]

## Primary Sources Checked

- Low and Wiebe, "Hamiltonian Simulation in the Interaction Picture", arXiv:1805.00675.

## Verdict

Minor issues. The note captures the norm-reduction idea well. It is somewhat too short for the theorem-level details and uses "exponential improvement" language that needs a precise parameter comparison.

## Line-Anchored Comments

- Lines 9-15: good description of the interaction-picture move. The paper is not merely a time-independent trick; the framework also supports time-dependent interaction-picture Hamiltonians. State the exact problem class.
- Lines 21-24: the two conditions for advantage are exactly right.
- Lines 28-34: the oracle-conjugation description is useful. It should mention that the cost of controlled `A` evolution enters every sampled-time query.
- Lines 38-44: the complexity formula is helpful but too schematic. Define `C_{e^{iA/alpha_B}}` more concretely and say whether the logarithmic factor comes from discretization/derivative bounds.
- Line 46: "exponential improvement" should be tied to a family where `alpha_A/alpha_B` grows exponentially and `A` is fast-forwardable. As a standalone statement, it sounds stronger than the theorem.
- Lines 50-54: the application table uses undefined symbols such as `alpha_T` and `alpha_B`. Add definitions or remove the table.
- Line 58: "quadratic improvement in query complexity" needs a theorem reference and the exact baseline; it is too compressed for review use.
- Lines 62-64: good caveats. They should be elevated because the entire result depends on the chosen split `H=A+B`.

## Missing or Suggested Additions

- Add the main theorem in the paper's own notation, including the interaction-picture smoothness/derivative parameters.
