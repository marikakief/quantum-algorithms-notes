# Tranche 16 Resolution

Resolved Tranche 16: Query Complexity / Lower Bounds / Search Variants.

## Summary

All substantive supervisor comments were either implemented or found already satisfied in the current notes. I did not reject any major correction. The largest change was a full rewrite of the convex-optimization note, because the previous note described the wrong algorithmic model for the cited paper.

## Notes Updated

- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]: added top-level title, made the total-function/query-model scope explicit, removed the misleading "sixth-root speedup" phrasing, and clarified later exponent caveats.
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]: added title, relabelled the method as positive/unweighted Ambainis adversary, separated the later negative/general adversary timeline, and corrected the element-distinctness cross-link.
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]: added title, restricted the equivalence and certificate barrier to positive/nonnegative adversary formulations, and softened the exact/zero-error claim.
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]]: added title, spelled out the oracle problem, and qualified the Shor comparison as a black-box abstraction rather than a direct query-complexity row.
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]: added title, clarified that `N=2^n` is truth-table size, corrected the total-function comparison, and made the combined-oracle query accounting explicit.
- [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]]: added title and repeated the average-case/advice-weighted qualifier. The exact-Grover promise and rank-order caveat were already present.
- [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes]]: added title, fixed log-base wording, removed the impossible Shannon-information derivation, and stated the paper-level ordered-search constant conservatively.
- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]: added title, fixed the corrupted binomial formula/control character, and labelled the lower bound as a positive weighted adversary argument.
- [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes]]: added title, put the main theorem and upper bound first, removed broad cubic-separation language, and replaced the classical comparison table with an access-model caveat.
- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]]: added title, made base-2 entropy explicit, defined serial codes, distinguished nonselective measurement entropy from selective branches, and fixed the QFA corollary row.
- [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]]: added title, made the constant-degree and whole-polynomial-learning qualifiers explicit, clarified finite-field character use, and added the `d=2` example.
- [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]]: added title and a model checklist; qualified the primitive-group, adjacency-matrix, and bounded-degree adjacency-list claims.
- [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]]: rewrote the note around the actual evaluation/membership-oracle convex-body result, corrected Xiaodi Wu's name, included the lower bounds and independent van Apeldoorn-Gilyén-Gribling-de Wolf result, and removed the wrong first-order/linear-system story.
- [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]: added title and emphasized query/copy complexity plus efficient implementation of the average measurement.
- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]: clarified sparse-oracle versus full fault-tolerant cost, one-solution output, and non-practical cryptanalytic status.
- [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]]: added resource terminology, marked the MNRS symbols, and labelled tradeoff formulas as fitted exponent ranges.
- [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]]: added title, separated exact degree from approximate degree, corrected the Špalek-Szegedy date, clarified `D(f)=3`, and separated later positive/negative adversary and approximate-degree developments.

## Verification

Scoped checks were run after the edits for stale high-risk phrases and control characters. Full whitespace and diff checks are recorded in the working log for this tranche.
