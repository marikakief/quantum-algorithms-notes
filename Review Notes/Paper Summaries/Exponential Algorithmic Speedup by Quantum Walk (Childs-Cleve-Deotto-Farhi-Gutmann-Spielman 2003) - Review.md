# Review: Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003)

Source note: [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]

## Primary Sources Checked

- Childs, Cleve, Deotto, Farhi, Gutmann, and Spielman, "Exponential algorithmic speedup by quantum walk," STOC 2003, arXiv:quant-ph/0209131.

## Verdict

Minor issues. This is a strong summary: it gets the welded-trees construction, the column-subspace reduction, and the oracle lower bound mostly right. The main edits should sharpen the cost accounting and avoid overcompressing the implementation assumptions.

## Line-Anchored Comments

- Lines 9-12: excellent high-level framing. Add "query/oracle" directly to the headline sentence so the exponential separation is not misread as an unconditional complexity separation.
- Lines 17-19: good explanation of why the random alternating cycle is needed. The contrast with the earlier glued-tree graph is clear.
- Lines 41-45: the column-subspace reduction and `gamma=1/sqrt(2)` normalization are right. It may be worth saying this is an invariant subspace for the particular initial state; arbitrary states do not reduce to the line.
- Lines 61-67: the success-probability statement should include the walk-time scale. Each run chooses time up to `Theta(n^4/epsilon)`, then repetition adds another `O(n)` factor for high success probability.
- Lines 71-88: the Hamiltonian implementation discussion is very useful. The edge-colouring/oracle-output-ordering assumption should be marked as part of the oracle model, not a free property of arbitrary black-box graphs.
- Lines 93-102: the classical lower-bound summary is good. State explicitly that the bound is over the random choice of the welded graph and random vertex labels, then converted into a hard distribution/oracle separation.
- Line 113: `BQP != BPP relative to this oracle` is acceptable shorthand, but the immediate object is a black-box traversal/search problem. A language-oracle formulation requires the usual packaging.
- Lines 120-123: the caveats are exactly the right ones. The "does not output a path" caveat is useful, but make clear that the computational task only asks for the EXIT name.
- Lines 149-157: cross-links are good and accurately place the oscillator speedup as inheriting the welded-trees lower-bound idea.

## Missing or Suggested Additions

- Add the total-polynomial cost in one place: simulation time, number of repetitions, and oracle/gate overhead.
- Add a one-line distinction between "quantum walk as physical Hamiltonian dynamics" and "implemented quantum circuit using graph oracle queries."

