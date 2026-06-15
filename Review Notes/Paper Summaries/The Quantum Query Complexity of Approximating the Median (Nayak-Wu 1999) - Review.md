# Review: The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999)

Source note: [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes]]

## Primary Sources Checked

- Nayak and Wu, "The quantum query complexity of approximating the median and related statistics", STOC 1999 / arXiv:quant-ph/9804066.

## Verdict

Major issues. The polynomial-method core is mostly right, but the "cubic separation" discussion is muddled and the comparison table overstates the classical side for approximate median.

## Line-Anchored Comments

- Add a top-level title.
- Lines 21-27: the main lower and upper bounds should be stated first as `Omega(min{1/epsilon,n})` and `O((1/epsilon) log(1/epsilon) log log(1/epsilon))`, respectively. That is the clean theorem.
- Line 23: "rare cubic quantum-classical separations" is not a safe headline. The theorem is a near-tight query bound in terms of `epsilon`; any separation claim depends on how `epsilon` scales with `n` and which classical model is being compared.
- Line 25: correct that exact median has no asymptotic quantum speedup over linear classical comparison complexity. This should not be followed by vague "cubic separation" language.
- Lines 75-81: the table is misleading. For constant `epsilon`, classical randomized algorithms can also sample to find an approximate median in sublinear query complexity in natural oracle models. If the paper's comparison-tree/worst-case formulation is intended, spell that out very carefully before writing `Theta(n)`.
- Line 81: "quantum `O(1)` vs classical `Theta(n)`" is the claim most in need of revision. It conflicts with the usual sampling intuition unless one is in a specific comparison/tree promise model; do not present it as a blanket classical lower bound.
- Lines 85-96: good algorithm sketch via approximate counting.
- Lines 117-118: the caveat notices the issue but is too weak. The main text should be rewritten, not merely caveated.
- Line 143: the adversary/certificate-barrier claim needs care. The Nayak-Wu lower bound is naturally a polynomial-method result; do not imply the positive adversary method is the right or only obstruction without a precise formulation.

## Missing or Suggested Additions

- Add a parameter table with `epsilon`, quantum queries, and the exact classical model. This paper is easy to misstate because "approximate median" has different classical behavior depending on access model and success criterion.

