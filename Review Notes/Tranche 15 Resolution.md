# Tranche 15 Resolution

Scope: HSP, nonabelian algebra, semidirect-product, solvable-group, and algebraic-number-theory notes reviewed in Tranche 15.

## Paper notes

- `The Hidden Subgroup Problem Review and Open Problems (Lomont 2004)`: implemented. Added the missing title, moved the 2004-date limitation into the opening, and clarified that the survey is best used for vocabulary and early landscape rather than current constants or post-2004 status. Updated the scope wording to avoid overreading solvable-group structural tasks as general HSP solutions.

- `The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004)`: implemented. Added the missing title, emphasized that exactification is a query-complexity construction with generally exponential probability-matrix/rotation implementation, distinguished hidden-subgroup state copies from efficient PGM implementation, and corrected the Watrous cross-link label.

- `On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998)`: implemented. Added the missing title, made the all-candidate maximization cost explicit as exponential in `log N`, and added the comparison between the paper's cosine-biased samples and Kuperberg's later labelled-qubit phase picture.

- `From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005)`: implemented. Added the missing title, scoped the cyclic-subgroup reduction, inserted a prose bridge before the dense hidden-state formula, and clarified that PGM implementation requires coherent matrix-sum solving/counting rather than a classical decision procedure. Added the Kuperberg contrast: PGM optimality is not measurement efficiency.

- `Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010)`: implemented major corrections. Added the missing title, softened the "most speedups" framing to the algebraic speedups covered by the survey, corrected Hallgren's continuous-period description, separated Hallgren 2002 from later class-group/unit-group algorithms, removed the false general solvable-group HSP claim, narrowed Regev's lattice implication to the relevant promised unique-SVP setting, and added a category table separating abelian HSP, normal/special nonabelian cases, solvable black-box structural tasks, and general finite-group query complexity.

- `Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002)`: implemented major corrections. Replaced the overbroad class-group corollary with Pell/regulator and PIP for real quadratic fields, added PIP output verification and success amplification language, moved broader class-group/unit-group algorithms to later work, rephrased the comparison with factoring as best-known-classical evidence rather than a formal separation, and distinguished Hallgren's regulator convention from Schmidt's narrow regulator.

- `Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009)`: implemented. Added why PIP needs two-dimensional Fourier sampling and added a Hallgren-vs-Schmidt table explaining the many-to-one fibre change, stored data, Fourier proof burden, and qubit-resource point. Kept the existing warning about the suspicious `polylog(log Delta)` notation.

- `Quantum Algorithms for Solvable Groups (Watrous 2001)`: implemented. Added the missing title and a top warning not to cite the paper as a general solvable-group HSP solver. The note now explicitly frames it as order, membership, equality, normality, and factor-group tools for solvable black-box groups.

- `Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001)`: implemented. Added the missing title and defined the runtime parameter `nu(G)` next to the theorem list, making the encoding/runtime assumptions visible at the result statements.

- `Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007)`: implemented. Added the missing title, explained why nonunique encoding is tolerated for `P_{p,r}` but not for the structured `m`-factor extension, and stated explicitly that the algorithmic theorem does not solve the dihedral or quasi-dihedral classes named by the classification theorem.

- `Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006)`: implemented. Added the missing title and attached the nontrivial order-`p` action and prime-factor restrictions directly to the theorem statement. The existing note already made clear that this is a decomposition result, not a new measurement primitive.

- `Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007)`: implemented. Added the missing title, rephrased the normal-subgroup fallback as known normal-HSP machinery for classified normal candidates rather than a general solvable-group HSP claim, added a decision table keyed by `(m,n)`, and narrowed the reusable-idea wording.

- `Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004)`: implemented. Added the missing title, added a group-theory note explaining "Hamiltonian", and clarified that `U_{G/M_G}(X)` means the subgroup generated by lifted cosets, not a bare set union. Existing caveats already kept the normal-HSP and subgroup-membership assumptions visible.

- `Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998)`: implemented. Added the missing title, emphasized that the Fourier basis/order choice is engineered for the wreath product and custom pairing, and added the comparison to affine-group basis-selection results.

- `An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007)`: implemented. Added the missing title, spelled out the `G' subset H` versus `G' cap H = {1}` recovery path, stated that the `j_i` are chosen after the Fourier labels are known, and added the small-prime caveat signaled by the witness-probability bound.

- `An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008)`: implemented. Added the missing title, explained why the classical reduction to trivial/order-`p` hidden subgroups suffices, emphasized that the Chevalley-Warning step must be constructive, and added an extraspecial-vs-nil-2 comparison table.

- `Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016)`: implemented. Added the missing title, distinguished shifted characteristic functions of known difference sets from injective hidden shift, tied Paley/Hadamard/Singer examples to efficient phase-correction assumptions, and kept the white-box/tiny-slice caveat for the dihedral-HSP application.

- `A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013)`: implemented. Added the missing title, fixed the success-parameter dependence to `log(1/epsilon)`, and added the bridge from coset states to the large affine irrep/wavelet component.
