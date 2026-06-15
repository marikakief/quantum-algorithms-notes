# Tranche 2 Resolution

Scope: phase estimation / amplitude amplification spine from `Review Notes/Review Index.md`.

## Summary

All substantive Tranche 2 supervisor suggestions were either already reflected in the current notes or implemented in this pass. I did not reject any correction outright. The only "keep current" cases were formula/counting-convention points that checked out against the source but needed clearer wording.

## Paper Notes

| Note | Resolution |
|---|---|
| `Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995)` | Already had the single-bit cost and multi-bit model caveats. Removed misleading ordinary-QPE links to `Gapped Phase Estimation`, separated Lemma 1/Lemma 2 reuse, and collapsed duplicate Deutsch references. |
| `Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998)` | Already softened invention claims and fixed QFT normalization. Clarified extra-bit cost convention and replaced ordinary-QPE links to `Gapped Phase Estimation` with iterative/semiclassical QPE links plus a SOSSA-specific caveat. |
| `Tight Bounds on Quantum Searching (Boyer-Brassard-Hoyer-Tapp 1998)` | Current note already distinguishes the `t=0` case, avoids postselection language in counting, cites later `pi/4` optimality work, and uses BBBV 1997. No further edit needed. |
| `Quantum Counting (Brassard-Hoyer-Tapp 1998)` | Current note already handles historical terminology, endpoints, `BHMT` spelling, and `8/pi^2` confidence. Tightened the reusable heuristic-speedup wording so it is not a blanket statement about arbitrary probabilistic algorithms. |
| `Quantum Amplitude Amplification and Estimation (Brassard-Hoyer-Mosca-Tapp 2002)` | Current note already fixed ordinary-vs-exact AA and endpoint-sensitive counting. Removed misleading `Gapped Phase Estimation` link from the references. |
| `Recursive Amplitude Amplification (Hoyer-Mosca-de Wolf)` | Current stub already had corrected metadata and theorem framing. Removed duplicate TODO line; full construction remains intentionally marked TODO. |
| `Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014)` | Current note already softened QSP lineage, exact-AA comparison, nesting/adaptivity, and fixed-point optimality. Source-checked the Chebyshev formula and nesting convention; retained them with clearer notation. |

## Trick Cards

| Card | Resolution |
|---|---|
| `Gapped Phase Estimation` | Reframed as a SOSSA-style interval classifier, not ordinary QPE. Corrected ancilla/control wording using Lemma 6: no phase-estimation register and no ancillas beyond the controlled-unitary/classifier qubit convention. |
| `Iterative Phase Estimation (Kitaev)` | Split Kitaev's original sine/cosine eigenvalue measurement from later semiclassical/feed-forward QPE. Rewrote overlap, bit-order, confidence, and shot-count caveats; removed unsourced `10^7` performance comparison. |
| `Amplitude Estimation via Phase Estimation on Q` | Corrected approximate-counting cost: additive error in `a=t/N` costs `O(1/epsilon)`, not `O(sqrt(N)/epsilon)`. Added a table separating additive, relative, and BHT/BHMT `P`-parameter regimes. |
| `Standard Amplitude Amplification` | Added coherent-randomness/reversibility requirements for classical heuristics, black-box optimality qualification, and unknown-`a` distinction between finding and estimating. |
| `Phase Estimation on the Grover Operator for Counting` | Replaced muddled relative-error statement with the BHT/BHMT bound `2*pi*sqrt(tN)/P + pi^2 N/P^2`, plus complement-counting caveat near `t=N`. |
| `Randomised Iteration Count for Grover Search` | Added candidate checking after measurement and the explicit `t>0` assumption / absence-certification caveat. |
| `Quadratic Speedup for Classical Heuristics via Amplitude Amplification` | Rewrote the opening and examples to require coherent seeds, reversible implementation, bounded-time or bucketed runtime, and a clean success predicate. |
| `Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification` | Source-checked Yoder-Low-Chuang Eq. (1); retained the formula but rewrote it with `gamma` notation and added the guaranteed-range caveat. |
| `Chebyshev Nesting (Semigroup Concatenation)` | Source-checked the semigroup/nesting equation and iterate count. Retained the `l1 + 2 l1 l2 + l2` convention, but defined it through `L_i=2l_i+1` and separated coherent prefix extension from measurement-based checking. |
