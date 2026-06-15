# Review: Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020)

Source note: [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]]

## Primary Sources Checked

- Ben-David, Childs, Gilyén, Kretschmer, Podder, and Wang, "Symmetries, graph properties, and quantum speedups," FOCS 2020, arXiv:2006.12760.

## Verdict

Minor issues. The note is strong and gives the right conceptual punchline: symmetry can suppress super-polynomial quantum speedups, but the answer depends on the query model. A few theorem statements need their model restrictions kept closer.

## Line-Anchored Comments

- Lines 17-25: excellent top-level summary. Keep "adjacency matrix" and "adjacency list" in every headline claim; otherwise the graph-property conclusion is easy to misquote.
- Lines 33-37: the well-shuffling proof sketch is good. The exponent `a`/power convention should be checked against the paper before quoting `R(f)=O(Q(f)^a)`; some statements hide constants or polynomial exponents in the definition of cost.
- Lines 43-51: the reduction from collision to graph symmetries is clearly presented. Good to preserve the directed/undirected distinction.
- Lines 55-63: the primitive-group dichotomy is accurate at a high level. It relies on CFSG/Liebeck and applies to primitive groups; do not present it as a complete classification of all permutation groups.
- Lines 67-84: the candy-graph construction is nicely explained. It should state the property-testing distance model and bounded-degree adjacency-list oracle explicitly.
- Lines 92-95: the result table is useful but slightly compressed. `Q(P_k)=poly(k), R(P_k)=2^{Omega(k)}` is a query-complexity statement for the constructed bounded-degree property family, not for all adjacency-list graph properties.
- Lines 103-106: good comparison with Aaronson-Ambainis, Chailloux, and Childs et al.; keep these because they orient the result historically.
- Lines 112-115: excellent caveats. The "natural graph property" caveat should be moved near the first mention of the adjacency-list exponential separation.

## Missing or Suggested Additions

- Add a "model checklist" near the top: partial Boolean function, query model, group action, primitive/imprimitive status, and whether the result is an upper bound or an existence construction.

