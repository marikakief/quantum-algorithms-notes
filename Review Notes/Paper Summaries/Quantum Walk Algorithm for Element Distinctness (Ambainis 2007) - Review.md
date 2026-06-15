# Review: Quantum Walk Algorithm for Element Distinctness (Ambainis 2007)

Source note: [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]

## Primary Sources Checked

- Ambainis, "Quantum walk algorithm for element distinctness", arXiv: https://arxiv.org/abs/quant-ph/0311001
- Shi, "Quantum lower bounds for the collision and the element distinctness problems", arXiv: https://arxiv.org/abs/quant-ph/0112086

## Verdict

Major issues in cross-link/lower-bound attribution; otherwise the algorithmic explanation is strong.

## Line-Anchored Comments

- Lines 13-19: comparison table

  Mostly correct in query-complexity terms. For ordinary element distinctness, the lower bound should be credited cleanly to Shi's polynomial-method result; "Aaronson-Shi" is common historical shorthand in some notes, but the arXiv source named here is Shi.

- Lines 33-43: Johnson graph setup

  Good. The key point is exactly that checking a stored subset is query-free, while moving to a neighboring subset costs only constant queries.

- Lines 57 and 68-72: query count

  Constants are suppressed. A walk step may use a query and an unquery; the asymptotic expression `r + N^{k/2}/r^{(k-1)/2}` is the important statement.

- Lines 78-100: invariant subspace/eigenvalues

  Useful and broadly source-faithful. The v9 arXiv has sign corrections on pages 11-12, so do not quote the eigenvalue formulas as implementation-ready without checking the final version.

- Lines 102-111: generalized Grover lemma

  Good. This is the right way to explain why an exact reflection about the starting state is not required.

- Lines 123-135: space-time tradeoff

  Correct for Ambainis's algorithmic family: `O(max(r, N/sqrt(r)))` for ED. Do not phrase it as a lower bound.

- Line 162: wrong linked reference

  The text says Boyer-Brassard-Hoyer-Tapp tight search, but the link points to the BHMT amplitude-amplification paper. Link to `[[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]`.

- Line 175: false lower-bound attribution

  This is the serious error. Ambainis's original adversary method does not prove the tight `Omega(N^{2/3})` lower bound for element distinctness; Shi's polynomial method does. The note itself says this correctly elsewhere, so fix the cross-link text.

## Missing or Suggested Additions

- Add a brief "positive vs negative witness" distinction if this note is later connected to learning graphs/adversary methods. The walk algorithm and the lower-bound method are different tools.
