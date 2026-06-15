# Tranche 3 Resolution

Scope: Shor arithmetic / collision / resource-estimate tranche from `Review Notes/Review Index.md`.

## Summary

Most supervisor comments were accepted and implemented directly. Two source-checked items were retained with clarified convention rather than removed: the Proos--Zalka reversible Euclidean quotient recovery and the Y-style birthday/Grover tradeoff. The latter was corrected from a lower-bound claim to an achieved algorithmic tradeoff.

## Paper Notes

| Note | Resolution |
|---|---|
| `A Quantum Algorithm for Finding the Minimum (Durr-Hoyer 1996)` | Added comparison-oracle model statement, fixed lower-bound attribution by reduction from search, clarified timeout constants/proof weighting, and replaced stale `Amplitude Amplification and Estimation` link. |
| `Quantum Algorithm for the Collision Problem (Brassard-Hoyer-Tapp 1997)` | Rewrote `ST^2` language as an achieved birthday-plus-Grover interpolation, not a lower bound. Added query-vs-runtime caveat, `r` dependence, table lookup caveat, and Shi/Aaronson--Shi lower-bound attribution. |
| `A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004)` | Added qubit convention separating clean ancilla from carry output, clarified NOT/depth counting convention, and scoped T-count discussion to later Clifford+T accounting. |
| `Factoring using 2n+2 qubits with Toffoli based modular multiplication (Haner-Roetteler-Svore 2017)` | Added `n` bit-length convention and corrected the Gidney--Ekerå RSA-2048 comparison from `3e8` to the source-consistent `~2.7e9` Toffoli+T/2 scale. |
| `Halving the Cost of Quantum Addition (Gidney 2018)` | Clarified 960 vs 5760 opportunity-cost crossover and rewrote "any circuit" language to require suitable compute/uncompute Toffoli pairs satisfying phase-insensitivity. |
| `Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003)` | Source-checked and corrected the reversible Euclidean quotient formula: after `a'=a-qb`, recover `q=floor(-a'/b)`. Softened one-run success phrasing and kept prime-field caveat. |
| `Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017)` | Added local parameter glossary, source-labeled the cryptographic resource table, normalized Unicode links, and left logical-vs-physical caveats already present. |
| `Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017)` | Added explicit warning about the paper's `(p+q-2)/2` convention versus the later Gidney--Ekerå `p+q` convention. Existing factor-recovery sentence was already correct. |
| `How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021)` | Fixed finite-field DLP notation from `Z_N^*` with `N` prime to `F_p^*` / `Z_p^*`; normalized Ekerå--Håstad links and source-labeled the headline physical table. |

## Trick Cards

| Card | Resolution |
|---|---|
| `MAJ-UMA Decomposition for Reversible Adders` | Added source wire-order convention, variant/counting caveats, and distinction between final-carry and modulo-`2^n` adders. |
| `Temporary Logical-AND` | Moved correctness condition above use cases, scoped replacement to suitable paired Toffolis, and distinguished Toffoli gates, temporary AND values, and persistent clean AND values. |
| `Dirty Ancilla Constant Addition` | Defined dirty/clean ancillae, scoped exact Toffoli constants to source conventions, and corrected controlled-version wording. |
| `Quantum Fourier Transform Circuit` | Rewrote gate notation to standard `H`/controlled-phase conventions, replaced fragile phase derivation with product-state factorization, and made depth model-dependent. |
| `Semiclassical QFT Register Recycling for Abelian Period Finding` | Added Griffiths--Niu attribution, precise applicability conditions, bit-order convention caveat, and physical feed-forward latency caveat. |
| `Reversible Modular Exponentiation with Garbage Cleanup` | Removed misleading "quantum watchdog" framing, added `gcd(x,N)=1` condition, and rewrote cleanup using concrete swap-and-inverse modular multiplication. |
| `Nested Windowed Modular Exponentiation for Shor Arithmetic` | Added unwindowed baseline and marked formulas as Gidney--Ekerå cost-model formulas tied to QROM, measurement-uncomputation, cosets, and carry runways. |
| `Classical Preprocessing plus Grover Search` | Rewrote solution count and table-access caveats; changed `ST^2` from lower-bound language to an achieved family relation. |
| `Birthday-Grover Space-Time Tradeoff` | Rewritten: no longer claims a universal `ST^2` lower bound; now separates BHT achieved tradeoff from Shi's query lower bound. |
| `Grover Search with Evolving Threshold for Minimum Finding` | Replaced heuristic summation with the source rank-visitation proof idea, added non-distinct-value caveat, and fixed stale link. |
| `Fixed-Base Order Finding for Reusable Factoring Circuits` | Scoped the trick to safe semiprimes, stated base-2 exclusions/promise, tied order constraints to the paper's lemma, and warned that random-base Shor remains standard for general composites. |
