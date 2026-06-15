# Review: Improved Error Bounds for the Adiabatic Approximation (Cheung-Hoyer-Wiebe 2011)

Source note: [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]

## Primary Sources Checked

- Cheung, Hoyer, and Wiebe, "Improved Error Bounds for the Adiabatic Approximation," Journal of Physics A: Mathematical and Theoretical 44, 415302 (2011) / arXiv:1103.4174.

## Verdict

Minor issues. This is a technically strong note. It mainly needs a few scope qualifiers around "certify" and around non-asymptotic versus asymptotic tightness.

## Line-Anchored Comments

- Lines 7-11: the overview is good, but "if you need to certify an adiabatic algorithm, this is the tool" is too broad. The theorem is finite-dimensional, non-degenerate, and differentiability-dependent.
- Lines 15-23: the error definition is clear. Add that the result is for leakage out of a chosen instantaneous eigenstate, not necessarily only ground-state algorithms.
- Lines 27-35: the JRS comparison is useful. Make clear which terms are loose asymptotically rather than implying the prior bound is invalid.
- Lines 53-75: the theorem summaries are readable. The "for any `T >= 0`" phrasing for the core bound should still carry the theorem's smoothness and nondegeneracy assumptions.
- Lines 97-115: the path-integral proof sketch is excellent and should remain. This is the note's strongest explanatory section.
- Lines 119-125: the Marzlin-Sanders discussion is good. Add that the issue is Hamiltonians depending on `T`; for fixed paths the usual intuition is less pathological.
- Lines 149-154: caveats are good. The nondegenerate assumption should also be mentioned in the computational-problem section.

## Missing or Suggested Additions

- Add a small notation table for `Delta_0`, `Delta_1`, `gamma_min`, and which norms are maxima over the path. The formulas are dense enough that readers will benefit from a local glossary.

