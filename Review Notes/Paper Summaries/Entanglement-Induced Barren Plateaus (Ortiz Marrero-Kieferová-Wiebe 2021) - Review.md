# Review: Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferova-Wiebe 2021)

Source note: [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]

## Verdict

Needs rewrite. The source note explicitly marks itself as a stub, and it is not yet a review-level paper summary. The headline is directionally right, but there is no theorem statement, no proof structure, no assumptions on the ansatz/cost/locality model, and no comparison to the McClean et al. barren-plateau mechanism beyond a few sentences.

## Line-Anchored Comments

- Lines 1-5: The stub warning is appropriate. Keep it until the note is replaced by a full treatment.

- Lines 7-18: Metadata is useful, but the live citation count should be avoided or dated very explicitly. Citation counts age and are not part of a stable technical summary.

- Lines 20-26: The key result needs a precise theorem statement. Specify the subsystem, parameter location, cost operator support, purity condition, and whether the bound is on gradient variance, average gradient magnitude, or absolute gradient.

- Lines 20-26: "Regardless of the specific circuit structure" is too broad unless the theorem's structural assumptions are stated. The paper's point is that entanglement/purity conditions imply barren plateaus, not that every entangling circuit in every training setup is covered.

- Lines 28-33: The significance bullets are fine as motivation, but they should be backed by the actual purity-gradient inequality and its scaling.

- Lines 36-40: The Jozsa-Linden connection is a good pedagogical bridge. It should not substitute for reviewing the paper itself.

- Lines 42-48: The TODO list identifies exactly what is missing. Add full derivation, examples, relation to local/global costs, and comparison with noise-induced and expressibility-induced barren plateaus.

## Missing or Suggested Additions

- Add the main theorem in notation from the paper.
- Add a proof sketch: gradient expression, reduced-state purity, concentration/variance bound, and scaling with subsystem size.
- Add a "what this does not say" section: entanglement can be necessary for expressivity while still harmful for trainability in some regimes.
