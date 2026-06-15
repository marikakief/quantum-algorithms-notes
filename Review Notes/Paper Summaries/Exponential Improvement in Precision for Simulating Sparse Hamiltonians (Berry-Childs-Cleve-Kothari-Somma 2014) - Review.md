# Review: Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014)

Source note: [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]]

## Primary Sources Checked

- Berry, Childs, Cleve, Kothari, and Somma, "Exponential improvement in precision for simulating sparse Hamiltonians", STOC 2014 / arXiv:1312.1414.

## Verdict

Minor issues. The note correctly presents this as the full OAA/fractional-query version and explains the lower bound well. A few details in the lower-bound and OAA lineage should be tightened.

## Line-Anchored Comments

- Lines 7-17: good summary and good historical placement relative to Berry-Cleve-Somma 2013 and the later Taylor-series paper.
- Lines 35-43: the fractional-query gadget is helpful, but the displayed phase convention for `Q^alpha` should be checked. There appears to be an extra global phase in the expression as written.
- Lines 57-71: excellent OAA explanation. The "2D subspace lemma" is the right conceptual point.
- Lines 77-80: the sparse-Hamiltonian reduction should mention that the `d^2` decomposition is the source of the remaining quadratic sparsity dependence.
- Lines 97-105: the lower-bound sketch is good, but line 103 says `(1/N)^N approx 1/N!`. These differ by an `e^N sqrt(N)` factor. The asymptotic inversion still gives `N = Theta(log(1/epsilon)/loglog(1/epsilon))`, so state that instead of using `approx`.
- Lines 123-131: useful reusable-idea list.
- Line 125: "introduced here; later made robust" is a bit ambiguous. This paper introduces OAA in the sparse/fractional-query setting; the later truncated-Taylor paper gives the clean robust-OAA formula for a slightly nonunitary truncated segment.
- Lines 148-150: "backbone of essentially all modern quantum algorithms" is too broad. It is central to LCU/block-encoding algorithms, but not every modern quantum algorithm uses OAA.

## Missing or Suggested Additions

- Include the exact simulation parameter `tau = d^2 ||H||_max t` near the verdict so readers do not confuse this result with the later `d ||H||_max t` Bessel/walk result.
