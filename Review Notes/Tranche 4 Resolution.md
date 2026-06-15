# Tranche 4 Resolution

Scope: quantum walk / element distinctness spine from `Review Notes/Review Index.md`.

## Summary

Most Tranche 4 supervisor comments were accepted and implemented. The main corrections were attribution and model control: Shi, not Ambainis's adversary method, is the source of the tight element-distinctness lower bound; generic quantum-walk search is not `O(sqrt(N/M))` without spectral-gap and cost parameters; and Johnson-graph subset search needs the `delta=Theta(1/r)` factor. I also source-checked the "current best" triangle-finding line and updated it to mention the later removal of dense-case logarithmic factors while keeping the `n^{5/4}` exponent.

## Paper Notes

| Note | Resolution |
|---|---|
| `Quantum Algorithms for Element Distinctness (Buhrman-Durr-Heiligman-Hoyer-Magniez-Santha-de Wolf 2001)` | Accepted. Clarified comparison-query versus value-oracle accounting, separated BHT promised collision finding from ordinary ED, changed the tight lower-bound row to Shi, and linked the search primitive to BBHT rather than the later BHMT amplitude-estimation note. |
| `Quantum Walk Algorithm for Element Distinctness (Ambainis 2007)` | Accepted. Changed the tight lower-bound attribution to Shi, made per-step query cost a suppressed constant rather than a literal net one-query operation, added the arXiv sign-correction caveat for eigenvalue formulas, fixed the BBHT link, and removed the false implication that Ambainis's adversary method proves the tight ED lower bound. |
| `Quantum Walks and Their Algorithmic Applications (Ambainis 2003)` | Accepted. Added a survey caveat, qualified grid-search table use, replaced the generic `O(sqrt(N/M))` framework with MNRS-style `S,U,C,delta,epsilon` parameters, and fixed the Grover reference link. |
| `Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004)` | Accepted. Reframed the theorem as marked-state detection, clarified uniform cardinal fraction versus later stationary marked probability, rewrote the bipartite-row notation, added the common `e^{±2i arccos(lambda)}` phase convention, softened qubitization-lineage wording, and distinguished Szegedy detection from MNRS finding. |
| `Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007)` | Accepted with a qualification. Replaced `epsilon=|M|/|X|` by stationary marked probability, cited Hoyer-Mosca-de Wolf recursive amplification directly, and softened the non-reversible-chain statement to require the discriminant/singular-value-gap assumptions. |
| `Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007)` | Accepted. Replaced an amplitude-estimation link with amplitude amplification/Grover search, dated the current-best comparison, added Jeffery-Kothari-Magniez's dense-case log-removal update, and weakened source-era adversary/lower-bound language. |
| `Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011)` | Accepted. Added the Belovs 2012 later-update caveat and comparison row, keeping clear that the 2011 result itself remains promised-input. The existing adversary-upper-bound explanation was already essentially correct. |

## Trick Cards

| Card | Resolution |
|---|---|
| `Walk on the Johnson Graph for Subset Search` | Accepted. Replaced the generic marked-vertex count with the MNRS template and explicitly inserted the Johnson-graph spectral gap `delta=Theta(1/r)`. Scoped `O(N^{k/(k+1)})` to Ambainis-style `k`-distinctness rather than all small-certificate subset problems. |
| `Generalised Grover Lemma for Quantum Walks` | Accepted. Added the two-layer Ambainis structure: take `Theta(sqrt r)` walk steps first to create a constant phase gap, then apply the generalized Grover iteration. |
| `Coined Quantum Walk Search on Graphs` | Accepted; partial rewrite. Removed the false universal `O(sqrt(N/M))` theorem, reframed the card around example-specific spectral analyses, and made marked-checking/query cost model-dependent. |
| `Recursive Quantum Walk for Nested Collision Problems` | Accepted. Added the adjacency-matrix query-model statement for the triangle-finding application. The known-subgraph caveat was already present and correct. |
| `Progressive Subtuple Enrichment in Learning Graphs` | Accepted. Added the Belovs 2012 unconditional update and cross-link to the typical-arc weighting card. |
| `Typical-Arc Reweighting for Almost-Symmetric Learning Graph Flows` | Accepted. Added local definitions for `mu(E)`, `pi(E)`, and `tau(E)`, plus the cross-link back to progressive subtuple enrichment. |
| `Nested Amplitude Amplification for Collision Search` | Accepted. Softened the log-factor claim to this comparison-model implementation and added a direct contrast with BHT's `r`-to-one promise. |

## Checks

- Targeted residual searches for the high-risk phrases from this tranche came back clean.
- `git diff --check` passed on all Tranche 4 touched files.
