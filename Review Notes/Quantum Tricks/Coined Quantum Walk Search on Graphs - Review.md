# Review: Coined Quantum Walk Search on Graphs

Source note: [[Coined Quantum Walk Search on Graphs]]

## Primary Sources Checked

- Ambainis, "Quantum walks and their algorithmic applications", arXiv: https://arxiv.org/abs/quant-ph/0403120
- Ambainis, Kempe, and Rivosh, "Coins make quantum walks faster", arXiv: https://arxiv.org/abs/quant-ph/0402107
- Shenvi, Kempe, and Whaley, "Quantum random-walk search algorithm", arXiv: https://arxiv.org/abs/quant-ph/0210064

## Verdict

Needs rewrite. The card presents a useful intuition but states a false generic theorem: coined walks do not find marked vertices on arbitrary graphs in `O(sqrt(N/M))` steps.

## Line-Anchored Comments

- Line 7: generic `O(sqrt(N/M))` claim

  Wrong as a general statement. This bound holds for some highly symmetric/search-friendly graphs and with the right coin/marked-vertex perturbation. General graph search depends on spectrum, dimension, connectivity, marked set placement, and success-amplification details.

- Lines 11-17: framework

  Good as a description of a coined walk state space and one common marked-vertex modification. It should be labeled as a framework, not a complete algorithmic theorem.

- Lines 21-25: Grover diffusion coin

  Mostly good. "Unique real unitary that treats all edges symmetrically" needs a qualifier: up to trivial phases/signs and under the specific symmetry assumptions.

- Lines 29-34: table

  Useful but too compressed. Grid results have dimension-specific log factors and success-probability issues; cite AKR directly in this card if keeping the table.

- Line 44: query cost

  A walk step does not always include one oracle query to check markedness. In many algorithms checking is implemented as a phase flip from stored data, and update/checking costs are separated. This is exactly why MNRS uses `S,U,C` notation.

- Lines 46-50: caveat

  Good. This caveat should move near the top and govern the whole card.

## Missing or Suggested Additions

- Rewrite around examples: hypercube search, grid search, Johnson-graph search. Then add a separate warning that there is no universal coined-walk Grover theorem for all graphs.
