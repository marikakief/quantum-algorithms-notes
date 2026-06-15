# Review: Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024)

Source note: [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]]

## Primary Sources Checked

- Berry et al., "Analyzing Prospects for Quantum Advantage in Topological Data Analysis," PRX Quantum 5, 010319 (2024), arXiv:2209.13581.

## Verdict

Minor-to-major issues. The note is excellent on algorithms, resource estimates, and dequantization, but it has two technical precision problems: a Betti-number dimension mismatch and a likely ambiguity between the Dirac gap and Laplacian gap in the Chebyshev-filter scaling.

## Line-Anchored Comments

- Lines 9-13: a `k`-clique corresponds to a `(k-1)`-simplex, and `beta_{k-1}` counts `(k-1)`-dimensional homology classes, not "k-dimensional holes." Fix the dimension language.
- Lines 17-19: the classical lower-bound statement is too broad without caveats. Later PIMC/dequantization results mean that "must enumerate all cliques" is not the right statement for all normalized/approximate regimes.
- Lines 23-31: the three-part summary is strong and balanced.
- Lines 45-49: the clique-state preparation section is clear. The "garbage is fine" point is important and should stay.
- Lines 65-69: check the definition of `lambda_min`. If it is the smallest nonzero eigenvalue of the Laplacian, then the Dirac operator has gap roughly `sqrt(lambda_min)`, so the filter cost should reflect the Dirac singular-value gap. If the paper defines `lambda_min` as the Dirac gap, say so explicitly.
- Lines 79-95: the complexity expression and simplified scaling are useful. Keep the `sqrt(Cl/beta)` factor visible; it is the main reason large Betti number matters.
- Lines 123-129: the speedup regimes are good. "Generic random graphs" at line 125 should be scoped to the specific Erdos-Renyi density window, not generic graphs in an everyday sense.
- Lines 133-143: the caveats are excellent. Line 133's "always exponential" for additive error should be softened to the high-dimensional regimes under discussion.
- Lines 147-153: the trick cards are appropriate, but the Chebyshev-filter card should carry the Dirac/Laplacian gap convention to avoid propagating the possible ambiguity.

## Missing or Suggested Additions

- Add a notation box distinguishing `k` vertices, `(k-1)`-simplices, `H_{k-1}`, `beta_{k-1}`, Dirac eigenvalues, and Laplacian eigenvalues.

