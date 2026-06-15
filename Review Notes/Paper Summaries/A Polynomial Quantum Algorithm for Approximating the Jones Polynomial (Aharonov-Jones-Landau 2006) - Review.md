# Review: A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006)

Source note: [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]

## Primary Sources Checked

- Aharonov, Jones, and Landau, "A Polynomial Quantum Algorithm for Approximating the Jones Polynomial," arXiv:quant-ph/0511096; Algorithmica 55, 395-421 (2009).

## Verdict

Minor-to-major issues. The algorithmic story is strong, but the note overgeneralizes classical hardness and should be more precise about additive normalization and the closure type for BQP-hardness.

## Line-Anchored Comments

- Lines 7-16: the problem setup is clear. "Exact evaluation is #P-hard at any non-trivial value" is too broad without listing exceptional roots/normalizations; Jones/Tutte exact-evaluation complexity has special easy points.
- Lines 21-25: good explanation of why AJL mattered: it made the TQFT/Jones algorithm explicit in ordinary quantum-circuit language.
- Lines 23-24: "most natural BQP-complete problem" is a defensible opinion, but mark it as opinion. Factoring, local Hamiltonian, and Jones have different kinds of naturalness.
- Lines 31-61: Temperley-Lieb/path-model explanation is good. The unitarity condition at roots of unity is the crucial algorithmic hinge.
- Lines 67-80: algorithm outline is useful. Be cautious with "each crossing contributes one 2-qubit gate"; the path encoding and locality/compilation details should be stated as efficient local gates, not necessarily a literal single two-qubit physical gate in every encoding.
- Lines 86-92: good theorem summary. The additive scale is the main caveat and should be stated before any "useful approximation" phrase.
- Lines 90-91: BQP-hardness is for plat closure in the specified parameter regimes. The note correctly says trace-closure hardness is not known later; keep that separation near the theorem list too.
- Lines 102-103: "classical algorithms all exponential" is too sweeping. Replace with a more precise statement: no polynomial-time classical algorithm is known for the BQP-hard additive approximations unless BQP collapses to BPP, while exact evaluation has known easy exceptional points.
- Lines 107-113: caveats are strong and should be moved closer to the one-line take if this note is used as an index entry.

## Missing or Suggested Additions

- Add a small table of closure type, normalization, additive scale, and known hardness.
- Add a one-sentence list of exceptional/easy Jones roots to avoid the "any non-trivial value" overclaim.

