# Review: Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007)

Source note: [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]

## Primary Sources Checked

- Aharonov, Arad, Eban, and Landau, "Polynomial Quantum Algorithms for Additive Approximations of the Potts Model and Other Points of the Tutte Plane," arXiv:quant-ph/0702008.

## Verdict

Minor issues. The note is detailed and conveys both the breadth and the catch: the algorithm covers broad planar Tutte/Potts parameters, but the additive window can be huge, especially for non-unitary representations.

## Line-Anchored Comments

- Lines 9-23: good setup. "Exact evaluation is #P-hard at essentially every non-trivial point" is acceptable only with "essentially"; do not remove that qualifier because the Tutte plane has exceptional curves/points.
- Lines 29-39: the two-part structure is well summarized. The non-unitary universality proof is unusually important here and the note gives it appropriate space.
- Lines 46-50: graph-to-Kauffman-bracket relation is clear. State that planarity is essential for this medial-graph/TL route.
- Lines 54-66: GTL explanation is one of the note's strengths. The "any representation works" phrase should be read inside the GTL scalar-coefficient construction, not as any arbitrary linear representation of any algebraic object.
- Lines 84-96: the algorithm and approximation scale are accurately described. The product-of-norms scale is the main reason the algorithm can be formally polynomial but practically uninformative.
- Lines 102-112: theorem table is useful. The BQP-completeness statement depends on the modified graph-plus-edge-partition formulation in the non-unitary case; keep that visible.
- Lines 150-164: non-unitary Solovay-Kitaev and grouping are summarized well. The grouping trick is artificial but central to matching hardness and algorithmic scales.
- Lines 168-172: excellent caveat on physical Potts parameters. This is the most important limitation and should remain prominent.
- Lines 189-195: caveats are complete. The embedding dependence and q=3 computer-search proof details are not throwaway issues.

## Missing or Suggested Additions

- Add a "what is physically meaningful?" paragraph separating formal BQP-complete complex/edge-partitioned instances from real positive Potts parameters.
- Add a one-line comparison of `Delta_alg` and `Delta_hard`, since most misunderstandings of this paper come from confusing those scales.

