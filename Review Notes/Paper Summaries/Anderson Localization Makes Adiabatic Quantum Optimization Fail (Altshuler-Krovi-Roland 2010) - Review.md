# Review: Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010)

Source note: [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]]

## Primary Sources Checked

- Altshuler, Krovi, and Roland, "Anderson localization makes adiabatic quantum optimization fail," Proceedings of the National Academy of Sciences 107(28), 12446-12450 (2010) / arXiv:0912.0746.

## Verdict

Minor-to-major issues. The note explains the mechanism well, but it should describe the result as a strong perturbative/physics argument rather than a fully rigorous typical-case theorem.

## Line-Anchored Comments

- Lines 15-19: the "typical-case negative result" framing is useful, but "with probability tending to 1" should be softened unless the note spells out the assumptions and non-rigorous steps in the physics argument.
- Lines 23-34: the Anderson-model mapping is good. "Exactly" is too strong because the cost-function energies are correlated by the clause structure, not independent onsite disorder.
- Lines 38-47: the anti-crossing argument is clearly summarized. Add that the cluster expansion and independence assumptions are part of the heuristic/statistical-mechanics machinery.
- Lines 49-59: the super-exponential gap estimate is central and should stay. Clarify that `N` here is the number of variables, while the Hilbert-space size is `2^N`; "super-exponential in `N`" can otherwise sound ambiguous.
- Lines 61-71: the comparison table is useful, but the final row should say "typical-case physics argument" rather than placing it on the same rigor footing as worst-case lower-bound constructions.
- Lines 73-79: the caveats are excellent and should be moved closer to the headline claim.

## Missing or Suggested Additions

- Add a "rigor status" paragraph: perturbative regime, localization assumption beyond `lambda_cr`, numerical coefficient extraction, and extension from EC3 to other NP-complete ensembles.

