# Review: Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005)

Source note: [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]

## Primary Sources Checked

- Morain, "Implementing the asymptotically fast version of the elliptic curve primality proving algorithm", arXiv:math/0502097 / Mathematics of Computation 76 (2007).

## Verdict

Minor issues. The note gets the fastECPP idea right and does a good job distinguishing the asymptotic trick from the implementation engineering. The main place to tighten is the exact status of the runtime assumptions.

## Line-Anchored Comments

- Lines 11-19: good cost-model setup with `L = log N`, `mu`, and `nu`.
- Lines 23-33: the square-root-pool explanation is clear and accurate.
- Lines 39-70: the ECPP skeleton is simplified but faithful enough. Add that real ECPP allows more general smooth cofactors `cN'`, not only the pedagogical `2N'` case.
- Lines 75-92: good accounting of why ordinary ECPP spends so much time on modular square roots.
- Lines 96-121: the fastECPP replacement is the key contribution. The phrase "pair version" is useful; keep it because the implementation can use richer combinations.
- Lines 127-155: excellent list of implementation details. Class invariants and smooth class numbers are not cosmetic; they control memory and root-finding constants.
- Lines 231-259: the complexity summary is good. The note should emphasize that after square roots are reduced, class-polynomial root finding and probable-prime tests become comparable bottlenecks.
- Lines 285-287: correct caveats. The even-class-number bias from composite discriminants is subtle and should remain.

## Missing or Suggested Additions

- Add a sentence distinguishing heuristic ECPP runtime from the deterministic validity of a completed ECPP certificate.
- The practical timing table is valuable, but say that the hardware-era timings are historical evidence for scaling, not current performance guidance.

