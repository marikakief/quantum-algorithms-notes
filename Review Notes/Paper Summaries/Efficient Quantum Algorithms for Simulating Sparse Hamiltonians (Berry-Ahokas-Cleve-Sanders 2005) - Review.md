# Review: Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005)

Source note: [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]

## Primary Sources Checked

- Berry, Ahokas, Cleve, and Sanders, "Efficient quantum algorithms for simulating sparse Hamiltonians", Communications in Mathematical Physics 2007 / arXiv:quant-ph/0508139.

## Verdict

Major issues. The core sparse-decomposition and Suzuki-order story is good, but the historical comparison table contains incorrect claims about later LCU/Taylor scaling.

## Line-Anchored Comments

- Lines 11-15: good separation between the sum-of-terms and sparse-black-box models.
- Lines 23-30: the main query-complexity summary is broadly right. It should define whether `||H||` means spectral norm or max-entry bound in the displayed formula, since later notes use `||H||_max`.
- Line 44: a color class in a bipartite edge-coloring is a matching, not necessarily a perfect matching.
- Lines 52-66: the diagram is helpful. It includes non-ASCII graphics; fine for Obsidian, but check rendering if publishing.
- Lines 84-101: the Suzuki optimization explanation is clear and useful.
- Lines 121-122: the lower-bound conclusion should be limited to the sparse black-box model. "Any black-box scheme" is okay only with the same oracle/access assumptions.
- Lines 131-135: the historical table has serious errors. Childs-Wiebe 2012 did not give "near-optimal via Taylor series"; the direct Taylor-series LCU algorithm is Berry-Childs-Cleve-Kothari-Somma 2015. Also line 134 has the Taylor scaling backwards: it should grow like `d^2 ||H||_max t * log(...)/loglog(...)`, not divide by `log(1/epsilon)`.
- Line 135: the QSP/qubitization row should use the additive precision term `tau + log(1/epsilon)/loglog(1/epsilon)` for Hamiltonian simulation, not a generic "O(tau + log)" statement.

## Missing or Suggested Additions

- Add the later Childs quantum-walk sparse simulation result with precise citation if it is going to appear in the historical table. Right now the table mixes published years, arXiv years, and technique families.
