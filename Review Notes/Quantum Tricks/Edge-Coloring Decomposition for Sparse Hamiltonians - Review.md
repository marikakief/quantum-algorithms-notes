# Review: Edge-Coloring Decomposition for Sparse Hamiltonians

Source note: [[Edge-Coloring Decomposition for Sparse Hamiltonians]]

## Primary Sources Checked

- Berry, Ahokas, Cleve, Sanders, arXiv:quant-ph/0508139: https://arxiv.org/abs/quant-ph/0508139
- BCCKS 2014 for the later bipartite sparse-decomposition refinement: https://arxiv.org/abs/1312.1414

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Line 12: "matching" is right; avoid saying "perfect matching" in related notes unless every vertex is covered.
- Lines 14-16: `6d^2` and `O(log* n)` are the BACS local-coloring result. Later sparse-simulation reductions modify this story, so the card should label this as the 2005 construction.
- Line 33: "best known local procedure" should be time-qualified or removed. It is not safe as a timeless statement in a vault that includes later sparse simulation.
- Line 49: LCU does not universally "avoid this decomposition entirely"; some LCU sparse encodings still need a sparse-oracle-to-unitary decomposition or other access-model conversion. Quantum-walk/qubitization approaches are the cleaner contrast.

## Missing or Suggested Additions

- Add a one-line note that the decomposition is implicit/oracle-based; the algorithm does not construct a full colored graph in memory.
