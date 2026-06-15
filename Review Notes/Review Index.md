# Review Index

Purpose: source-checked professor-style review annotations for the Obsidian vault. Original student notes are not edited here.

## Scope Inventory

- Paper summaries discovered: 409 Markdown files in `Paper Summaries/`
- Trick cards discovered: 1,060 Markdown files in `Quantum Tricks/`
- README counts are stale for this checkout.

## Status Legend

- `sound`: no substantive correction found in this pass
- `minor issues`: wording, attribution, notation, or missing caveats
- `major issues`: a claim is materially wrong, overstated, or likely to mislead
- `needs rewrite`: the note's central account is unreliable

## Completed Tranche 1 - Foundational / Oracle Spine

| Note | Verdict | Review file |
|---|---:|---|
| [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Simulating Physics with Computers (Feynman 1982) - Review]] |
| [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) - Review]] |
| [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) - Review]] |
| [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Complexity Theory (Bernstein-Vazirani 1993) - Review]] |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/On the Power of Quantum Computation (Simon 1994) - Review]] |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) - Review]] |
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) - Review]] |
| [[Phase Kickback from Oracle Queries]] | minor issues | [[Review Notes/Quantum Tricks/Phase Kickback from Oracle Queries - Review]] |
| [[Hadamard Sandwich for Global Function Properties]] | minor issues | [[Review Notes/Quantum Tricks/Hadamard Sandwich for Global Function Properties - Review]] |
| [[Superposition Query for Global Properties]] | major issues | [[Review Notes/Quantum Tricks/Superposition Query for Global Properties - Review]] |
| [[Coset Sampling via Fourier Transform]] | major issues | [[Review Notes/Quantum Tricks/Coset Sampling via Fourier Transform - Review]] |
| [[Interference-Based Period Finding]] | major issues | [[Review Notes/Quantum Tricks/Interference-Based Period Finding - Review]] |
| [[Order-Finding via QFT and Continued Fractions]] | minor issues | [[Review Notes/Quantum Tricks/Order-Finding via QFT and Continued Fractions - Review]] |
| [[Inversion About the Mean]] | minor issues | [[Review Notes/Quantum Tricks/Inversion About the Mean - Review]] |

## Tranche 1 Pattern Notes

- The student often writes in a strong explanatory voice. That is useful, but it becomes risky when historical or model-specific claims are stated as theorem-level facts.
- Repeated problem: original-paper claims are blended with later modern presentations. This is clearest in Deutsch/Deutsch-Jozsa phase-kickback accounts, Grover/amplitude-amplification framing, and Shor resource accounting.
- Repeated notation problem: `N` alternates between an integer to be factored, input bit length, and search-space size. Several complexity statements need explicit parameter conventions.
- Repeated HSP problem: Simon's exact abelian coset sampling is being used as if it transfers unchanged to Shor and to arbitrary abelian/nonabelian HSPs. Shor's order-finding uses an approximate Fourier-sampling analysis over a power-of-two register, not exact sampling from `Z_N/H` unless the period divides the register size.

## Completed Tranche 2 - Phase Estimation / Amplitude Amplification Spine

| Note | Verdict | Review file |
|---|---:|---|
| [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) - Review]] |
| [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) - Review]] |
| [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) - Review]] |
| [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Counting (Brassard-Hoyer-Tapp 1998) - Review]] |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Amplitude Amplification and Estimation (Brassard-Hoyer-Mosca-Tapp 2002) - Review]] |
| [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)]] | needs rewrite | [[Review Notes/Paper Summaries/Recursive Amplitude Amplification (Hoyer-Mosca-de Wolf) - Review]] |
| [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) - Review]] |
| [[Gapped Phase Estimation]] | major issues | [[Review Notes/Quantum Tricks/Gapped Phase Estimation - Review]] |
| [[Iterative Phase Estimation (Kitaev)]] | major issues | [[Review Notes/Quantum Tricks/Iterative Phase Estimation (Kitaev) - Review]] |
| [[Amplitude Estimation via Phase Estimation on Q]] | minor issues | [[Review Notes/Quantum Tricks/Amplitude Estimation via Phase Estimation on Q - Review]] |
| [[Standard Amplitude Amplification]] | minor issues | [[Review Notes/Quantum Tricks/Standard Amplitude Amplification - Review]] |
| [[Phase Estimation on the Grover Operator for Counting]] | minor issues | [[Review Notes/Quantum Tricks/Phase Estimation on the Grover Operator for Counting - Review]] |
| [[Randomised Iteration Count for Grover Search]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Randomised Iteration Count for Grover Search - Review]] |
| [[Quadratic Speedup for Classical Heuristics via Amplitude Amplification]] | major issues | [[Review Notes/Quantum Tricks/Quadratic Speedup for Classical Heuristics via Amplitude Amplification - Review]] |
| [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]] | minor issues | [[Review Notes/Quantum Tricks/Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification - Review]] |
| [[Chebyshev Nesting (Semigroup Concatenation)]] | minor issues | [[Review Notes/Quantum Tricks/Chebyshev Nesting (Semigroup Concatenation) - Review]] |

## Tranche 2 Pattern Notes

- Several notes conflate Kitaev's original eigenvalue-measurement procedure with the later textbook inverse-QFT phase-estimation circuit and still later semiclassical/iterative QPE circuits.
- The amplitude-amplification notes repeatedly blur "high probability" with "certainty". Exact certainty requires adjusted phases or additional conditions; ordinary Grover/BHMT iteration does not generally land exactly on the good subspace.
- Counting and amplitude-estimation cards need cleaner separation of additive error in probability `a`, additive error in count `t`, relative error in `t`, and endpoint cases `t=0,N`.
- The Høyer-Mosca-de Wolf recursive-amplification stub has wrong metadata: the relevant paper is `Quantum Search on Bounded-Error Inputs`, arXiv `quant-ph/0304052`, ICALP 2003.

## Completed Tranche 3 - Shor Arithmetic / Collision / Resource Estimates

| Note | Verdict | Review file |
|---|---:|---|
| [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Quantum Algorithm for Finding the Minimum (Durr-Hoyer 1996) - Review]] |
| [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithm for the Collision Problem (Brassard-Hoyer-Tapp 1997) - Review]] |
| [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) - Review]] |
| [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Factoring using 2n+2 qubits with Toffoli based modular multiplication (Haner-Roetteler-Svore 2017) - Review]] |
| [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Halving the Cost of Quantum Addition (Gidney 2018) - Review]] |
| [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) - Review]] |
| [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) - Review]] |
| [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekera-Hastad 2017) - Review]] |
| [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) - Review]] |
| [[MAJ-UMA Decomposition for Reversible Adders]] | minor issues | [[Review Notes/Quantum Tricks/MAJ-UMA Decomposition for Reversible Adders - Review]] |
| [[Temporary Logical-AND]] | minor issues | [[Review Notes/Quantum Tricks/Temporary Logical-AND - Review]] |
| [[Dirty Ancilla Constant Addition]] | minor issues | [[Review Notes/Quantum Tricks/Dirty Ancilla Constant Addition - Review]] |
| [[Quantum Fourier Transform Circuit]] | major issues | [[Review Notes/Quantum Tricks/Quantum Fourier Transform Circuit - Review]] |
| [[Semiclassical QFT Register Recycling for Abelian Period Finding]] | minor issues | [[Review Notes/Quantum Tricks/Semiclassical QFT Register Recycling for Abelian Period Finding - Review]] |
| [[Reversible Modular Exponentiation with Garbage Cleanup]] | major issues | [[Review Notes/Quantum Tricks/Reversible Modular Exponentiation with Garbage Cleanup - Review]] |
| [[Nested Windowed Modular Exponentiation for Shor Arithmetic]] | minor issues | [[Review Notes/Quantum Tricks/Nested Windowed Modular Exponentiation for Shor Arithmetic - Review]] |
| [[Classical Preprocessing plus Grover Search]] | minor issues | [[Review Notes/Quantum Tricks/Classical Preprocessing plus Grover Search - Review]] |
| [[Birthday-Grover Space-Time Tradeoff]] | needs rewrite | [[Review Notes/Quantum Tricks/Birthday-Grover Space-Time Tradeoff - Review]] |
| [[Grover Search with Evolving Threshold for Minimum Finding]] | minor issues | [[Review Notes/Quantum Tricks/Grover Search with Evolving Threshold for Minimum Finding - Review]] |
| [[Fixed-Base Order Finding for Reusable Factoring Circuits]] | minor issues | [[Review Notes/Quantum Tricks/Fixed-Base Order Finding for Reusable Factoring Circuits - Review]] |

## Tranche 3 Pattern Notes

- The arithmetic/resource notes are generally stronger than the foundational-oracle notes, but they are vulnerable to parameter drift: `N` as modulus, `n` as bit length, exponent length `n_e`, table size, and group order are sometimes mixed.
- Several cards state algorithmic tradeoffs as lower bounds. The worst instance is `[[Birthday-Grover Space-Time Tradeoff]]`, which turns BHT's achieved birthday/Grover interpolation into a universal `ST^2` lower bound.
- The QFT-related cards need tighter convention control. Bit reversal, qubit order, semiclassical measurement order, and approximation error are all places where the note text can look right while encoding the wrong circuit.
- Temporary logical-AND is mostly understood, but the notes need to keep Gidney's phase-insensitivity condition attached to every "replace paired Toffolis" claim.
- Resource-estimate comparisons should be source-labeled and recalculated before quoting. One line in the HRS note quotes Gidney-Ekera's RSA-2048 Toffoli count about a factor of ten too low.

## Completed Tranche 4 - Quantum Walk / Element Distinctness Spine

| Note | Verdict | Review file |
|---|---:|---|
| [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Element Distinctness (Buhrman-Durr-Heiligman-Hoyer-Magniez-Santha-de Wolf 2001) - Review]] |
| [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) - Review]] |
| [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Walks and Their Algorithmic Applications (Ambainis 2003) - Review]] |
| [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) - Review]] |
| [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) - Review]] |
| [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) - Review]] |
| [[Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) - Review]] |
| [[Walk on the Johnson Graph for Subset Search]] | major issues | [[Review Notes/Quantum Tricks/Walk on the Johnson Graph for Subset Search - Review]] |
| [[Generalised Grover Lemma for Quantum Walks]] | minor issues | [[Review Notes/Quantum Tricks/Generalised Grover Lemma for Quantum Walks - Review]] |
| [[Coined Quantum Walk Search on Graphs]] | needs rewrite | [[Review Notes/Quantum Tricks/Coined Quantum Walk Search on Graphs - Review]] |
| [[Recursive Quantum Walk for Nested Collision Problems]] | minor issues | [[Review Notes/Quantum Tricks/Recursive Quantum Walk for Nested Collision Problems - Review]] |
| [[Progressive Subtuple Enrichment in Learning Graphs]] | minor issues | [[Review Notes/Quantum Tricks/Progressive Subtuple Enrichment in Learning Graphs - Review]] |
| [[Typical-Arc Reweighting for Almost-Symmetric Learning Graph Flows]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Typical-Arc Reweighting for Almost-Symmetric Learning Graph Flows - Review]] |
| [[Nested Amplitude Amplification for Collision Search]] | minor issues | [[Review Notes/Quantum Tricks/Nested Amplitude Amplification for Collision Search - Review]] |

## Tranche 4 Pattern Notes

- Walk-search claims repeatedly overgeneralize from examples and surveys to theorem-level generic `O(sqrt(N/M))` statements. The safe vocabulary is setup cost `S`, update cost `U`, checking cost `C`, spectral gap `delta`, and marked stationary measure `epsilon`.
- Lower-bound attributions are fragile in this cluster. In particular, the tight element-distinctness lower bound is Shi's polynomial-method result, not Ambainis's adversary method.
- Several links still route through BHMT/amplitude-amplification notes where BBHT or ordinary Grover search is the actual citation.
- The learning-graph notes are comparatively careful, but Belovs-Lee should be explicitly contextualized as a prior-knowledge result later subsumed by Belovs's 2012 unconditional `k`-distinctness algorithm.

## Completed Tranche 5 - Spatial / Continuous-Time Quantum Walks and Graph Search

| Note | Verdict | Review file |
|---|---:|---|
| [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) - Review]] |
| [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] | sound with minor caveats | [[Review Notes/Paper Summaries/Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) - Review]] |
| [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Spatial Search by Quantum Walk (Childs-Goldstone 2004) - Review]] |
| [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Universal Computation by Quantum Walk (Childs 2009) - Review]] |
| [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] | needs rewrite | [[Review Notes/Paper Summaries/On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) - Review]] |
| [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Any AND-OR Formula Can Be Evaluated in O(N half plus o(1)) Queries (Ambainis-Childs-Reichardt-Spalek-Zhang 2007) - Review]] |
| [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) - Review]] |
| [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) - Review]] |
| [[Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) - Review]] |
| [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) - Review]] |
| [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) - Review]] |
| [[Continuous-Time Quantum Walk Search]] | minor issues | [[Review Notes/Quantum Tricks/Continuous-Time Quantum Walk Search - Review]] |
| [[Avoided Crossing at Critical Hopping Rate]] | major issues | [[Review Notes/Quantum Tricks/Avoided Crossing at Critical Hopping Rate - Review]] |
| [[Symmetry Reduction to Column Subspace in Quantum Walks]] | major issues | [[Review Notes/Quantum Tricks/Symmetry Reduction to Column Subspace in Quantum Walks - Review]] |
| [[Ballistic Quantum Transport via Bessel Functions]] | minor issues | [[Review Notes/Quantum Tricks/Ballistic Quantum Transport via Bessel Functions - Review]] |
| [[Column-Subspace Reduction for Symmetric Graphs]] | minor issues | [[Review Notes/Quantum Tricks/Column-Subspace Reduction for Symmetric Graphs - Review]] |
| [[Oracle-to-Hamiltonian Simulation via Swap Conjugation]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Oracle-to-Hamiltonian Simulation via Swap Conjugation - Review]] |
| [[Gate Implementation via Graph Scattering]] | minor issues | [[Review Notes/Quantum Tricks/Gate Implementation via Graph Scattering - Review]] |
| [[Momentum Filtering on Graphs]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Momentum Filtering on Graphs - Review]] |
| [[Hamiltonian Oracle to Discrete Query Conversion via Product Formulas]] | major issues | [[Review Notes/Quantum Tricks/Hamiltonian Oracle to Discrete Query Conversion via Product Formulas - Review]] |
| [[Szegedy Walk for Formula Evaluation]] | minor issues | [[Review Notes/Quantum Tricks/Szegedy Walk for Formula Evaluation - Review]] |
| [[Quantum Walk on Backtracking Trees via Effective Resistance]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Quantum Walk on Backtracking Trees via Effective Resistance - Review]] |
| [[Eigenvector-Encoded Path for Unique Search]] | minor issues | [[Review Notes/Quantum Tricks/Eigenvector-Encoded Path for Unique Search - Review]] |
| [[Interpolated Walk for Quantum Search]] | major issues | [[Review Notes/Quantum Tricks/Interpolated Walk for Quantum Search - Review]] |
| [[Incremental Eigenvalue Estimation]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Incremental Eigenvalue Estimation - Review]] |
| [[Discriminant Matrix Spectral Theorem]] | minor issues | [[Review Notes/Quantum Tricks/Discriminant Matrix Spectral Theorem - Review]] |

## Tranche 5 Pattern Notes

- Continuous-time walk notes repeatedly drift between adjacency-Hamiltonian and Laplacian/generator conventions. This is harmless only when the sign convention is explicitly tracked; otherwise "ground state", diagonal terms, and avoided-crossing claims become misleading.
- Several spatial-search entries mix raw Hamiltonian evolution time with the total cost of finding a marked vertex at constant probability. Low-dimensional cases especially need success probability carried through the complexity statement.
- The KMO interpolated-walk notes contain a central algebra error: the balancing parameter should be derived from `(1-s)(1-p_M)=p_M`, not with a square root.
- Childs 2010 needs a technical rewrite: the Perron-vector ratio in the walk-state construction appears inverted, and the glued-trees example is not an `abs(H)` sign-problem example.
- The formula/NAND-tree summaries are strong overall, but metadata and model labels need tightening: FGG's runtime is `O(sqrt(N log N))`, Yonge-Mallo is misspelled in one line, and the product-formula conversion is not a general gate-efficient simulation theorem.

## Completed Tranche 6 - Hamiltonian Simulation Spine

| Note | Verdict | Review file |
|---|---:|---|
| [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Universal Quantum Simulators (Lloyd 1996) - Review]] |
| [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) - Review]] |
| [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) - Review]] |
| [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/LCU Origins (Childs-Wiebe 2012) - Review]] |
| [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) - Review]] |
| [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) - Review]] |
| [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) - Review]] |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) - Review]] |
| [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) - Review]] |
| [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] | needs rewrite | [[Review Notes/Paper Summaries/Hamiltonian Simulation by Qubitization (Low-Chuang 2019) - Review]] |
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/QSVT and Beyond (Gilyen et al. 2018-2019) - Review]] |
| [[Product-Formula Time-Slicing for Local Hamiltonians]] | major issues | [[Review Notes/Quantum Tricks/Product-Formula Time-Slicing for Local Hamiltonians - Review]] |
| [[Suzuki Order as a Tunable Knob for Time Scaling]] | minor issues | [[Review Notes/Quantum Tricks/Suzuki Order as a Tunable Knob for Time Scaling - Review]] |
| [[Order-Condition Cancellation in Product Formulas]] | minor issues | [[Review Notes/Quantum Tricks/Order-Condition Cancellation in Product Formulas - Review]] |
| [[Trotter Commutator-Scaling Bound]] | major issues | [[Review Notes/Quantum Tricks/Trotter Commutator-Scaling Bound - Review]] |
| [[Edge-Coloring Decomposition for Sparse Hamiltonians]] | minor issues | [[Review Notes/Quantum Tricks/Edge-Coloring Decomposition for Sparse Hamiltonians - Review]] |
| [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]] | minor issues | [[Review Notes/Quantum Tricks/1-Sparse Hamiltonian Simulation via 2x2 Blocks - Review]] |
| [[Linear Combination of Unitaries (LCU)]] | major issues | [[Review Notes/Quantum Tricks/Linear Combination of Unitaries (LCU) - Review]] |
| [[Oblivious Amplitude Amplification (Robust)]] | minor issues | [[Review Notes/Quantum Tricks/Oblivious Amplitude Amplification (Robust) - Review]] |
| [[Taylor Series Truncation with ln2 Segmentation]] | minor issues | [[Review Notes/Quantum Tricks/Taylor Series Truncation with ln2 Segmentation - Review]] |
| [[Truncated Taylor Series Simulation]] | needs rewrite | [[Review Notes/Quantum Tricks/Truncated Taylor Series Simulation - Review]] |
| [[Standard-Form Encoding (Prepare + Signal Oracle)]] | major issues | [[Review Notes/Quantum Tricks/Standard-Form Encoding (Prepare + Signal Oracle) - Review]] |
| [[Block-Encoding Composition Algebra]] | minor issues | [[Review Notes/Quantum Tricks/Block-Encoding Composition Algebra - Review]] |
| [[Qubitization (Quantum Walk for Spectral Encoding)]] | major issues | [[Review Notes/Quantum Tricks/Qubitization (Quantum Walk for Spectral Encoding) - Review]] |
| [[Qubitization Iterate]] | major issues | [[Review Notes/Quantum Tricks/Qubitization Iterate - Review]] |
| [[QSVT Meta-Template]] | minor issues | [[Review Notes/Quantum Tricks/QSVT Meta-Template - Review]] |
| [[Jacobi-Anger Truncation for QSP Simulation]] | sound with minor caveats | [[Review Notes/Quantum Tricks/Jacobi-Anger Truncation for QSP Simulation - Review]] |
| [[Signal Transduction via Controlled-W]] | minor issues | [[Review Notes/Quantum Tricks/Signal Transduction via Controlled-W - Review]] |
| [[Phased Qubitization Sequence]] | minor issues | [[Review Notes/Quantum Tricks/Phased Qubitization Sequence - Review]] |

## Tranche 6 Pattern Notes

- The Hamiltonian-simulation notes are strongest when they explain algorithmic architecture, but weakest when they suppress normalization. Standard-form/block-encoding cards should always display `H/alpha`, not just `H`.
- Several notes conflate raw physical time `t`, normalized time `alpha t` or `lambda t`, sparse-model `d ||H||max t`, and LCU coefficient 1-norm time. This is the main technical problem in the tranche.
- Taylor/LCU versus QSP/qubitization comparisons repeatedly misstate the precision improvement. The important change is additive precision dependence, not simply "removing a loglog factor" or replacing everything by raw `O(t + log(1/epsilon))`.
- OAA and LCU cards need stricter hypotheses: nonnegative prepared weights, phases absorbed into controlled branches, success probability depending on `||tilde U |psi>||`, and uniform success amplitude for oblivious amplification.
- Product-formula cards need to distinguish fixed-order, optimized Suzuki, commutator-aware, randomized, and multiproduct analyses. "Trotter is polynomial in `1/epsilon`" is only a fixed-order statement.

## Completed Tranche 7 - Modern Product-Formula Refinements

| Note | Verdict | Review file |
|---|---:|---|
| [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) - Review]] |
| [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) - Review]] |
| [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) - Review]] |
| [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) - Review]] |
| [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) - Review]] |
| [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] | needs rewrite | [[Review Notes/Paper Summaries/Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) - Review]] |
| [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) - Review]] |
| [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) - Review]] |
| [[Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) - Review]] |
| [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) - Review]] |
| [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) - Review]] |
| [[Efficient and Practical Hamiltonian Simulation from Time-Dependent Product Formulas (Bosse-Childs-Derby-Gambetta-Montanaro-Santos 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient and Practical Hamiltonian Simulation from Time-Dependent Product Formulas (Bosse-Childs-Derby-Gambetta-Montanaro-Santos 2025) - Review]] |
| [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]] | minor issues | [[Review Notes/Quantum Tricks/Term-Weighted Random Hamiltonian Sampling (qDRIFT) - Review]] |
| [[Fixed-Angle Randomized Product Formula]] | minor issues | [[Review Notes/Quantum Tricks/Fixed-Angle Randomized Product Formula - Review]] |
| [[Diamond-Norm Channel Averaging for Random Compilers]] | major issues | [[Review Notes/Quantum Tricks/Diamond-Norm Channel Averaging for Random Compilers - Review]] |
| [[Permutation Averaging for Product-Formula Error Cancellation]] | major issues | [[Review Notes/Quantum Tricks/Permutation Averaging for Product-Formula Error Cancellation - Review]] |
| [[Randomized Multi-Product Sampling]] | needs rewrite | [[Review Notes/Quantum Tricks/Randomized Multi-Product Sampling - Review]] |
| [[Resolution Factor Minimization for Multi-Product Formulas]] | minor issues | [[Review Notes/Quantum Tricks/Resolution Factor Minimization for Multi-Product Formulas - Review]] |
| [[Processed Product Formula Kernel-Processor Decomposition]] | major issues | [[Review Notes/Quantum Tricks/Processed Product Formula Kernel-Processor Decomposition - Review]] |
| [[Eigenvalue Error as the Correct Product Formula Metric]] | minor issues | [[Review Notes/Quantum Tricks/Eigenvalue Error as the Correct Product Formula Metric - Review]] |
| [[Recursive Group-Commutator Product Formula]] | minor issues | [[Review Notes/Quantum Tricks/Recursive Group-Commutator Product Formula - Review]] |
| [[Operator Differential Method for Commutator Product Formulas]] | minor issues | [[Review Notes/Quantum Tricks/Operator Differential Method for Commutator Product Formulas - Review]] |
| [[Symplectic Corrector Injection for Product Formulas]] | minor issues | [[Review Notes/Quantum Tricks/Symplectic Corrector Injection for Product Formulas - Review]] |
| [[Optimal-Order Product Formula Selection]] | needs rewrite | [[Review Notes/Quantum Tricks/Optimal-Order Product Formula Selection - Review]] |

## Tranche 7 Pattern Notes

- Randomized simulation notes must separate three different objects: an individual sampled circuit, the averaged quantum channel, and a coherent unitary approximation. Several current cards slide between them.
- The randomized multi-product notes likely contain a central estimator error: observable estimation for a coherent linear combination requires cross terms, not just a single sampled product-formula branch.
- Product-formula optimization notes are strong when they discuss constants and empirical regimes, but several rhetorical claims overstate "8th order is optimal" or "10th order is irrelevant" beyond the tested cost model.
- Several product-formula cards use "order of magnitude", "order of approximation", and "order of error" interchangeably. These should be made precise because papers in this cluster use all three phrases differently.
- The corrected-product-formula and THRIFT notes are comparatively careful; the main risk is comparing them without specifying which primitives are available (`e^{A}`, `e^{B}`, `e^{A+alpha B_j}`, or time-ordered interaction-picture evolutions).

## Completed Tranche 8 - Early Quantum Chemistry / Fermionic Simulation

| Note | Verdict | Review file |
|---|---:|---|
| [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) - Review]] |
| [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) - Review]] |
| [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) - Review]] |
| [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) - Review]] |
| [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) - Review]] |
| [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) - Review]] |
| [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) - Review]] |
| [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) - Review]] |
| [[Jordan-Wigner Transformation for Chemistry Hamiltonians]] | major issues | [[Review Notes/Quantum Tricks/Jordan-Wigner Transformation for Chemistry Hamiltonians - Review]] |
| [[Fermionic SWAP for Anticommutation Enforcement]] | minor issues | [[Review Notes/Quantum Tricks/Fermionic SWAP for Anticommutation Enforcement - Review]] |
| [[Fermionic Swap Network]] | major issues | [[Review Notes/Quantum Tricks/Fermionic Swap Network - Review]] |
| [[Trotterized Time Evolution for Chemistry]] | major issues | [[Review Notes/Quantum Tricks/Trotterized Time Evolution for Chemistry - Review]] |
| [[Plane-Wave Dual Basis]] | minor issues | [[Review Notes/Quantum Tricks/Plane-Wave Dual Basis - Review]] |
| [[First-Quantized Plane-Wave Chemistry Encoding]] | major issues | [[Review Notes/Quantum Tricks/First-Quantized Plane-Wave Chemistry Encoding - Review]] |
| [[Split-Operator Real-Space Simulation on a Quantum Computer]] | minor issues | [[Review Notes/Quantum Tricks/Split-Operator Real-Space Simulation on a Quantum Computer - Review]] |
| [[Real-Space Grid Encoding for Many-Body Simulation]] | minor issues | [[Review Notes/Quantum Tricks/Real-Space Grid Encoding for Many-Body Simulation - Review]] |
| [[On-the-Fly Coherent Molecular Integral Computation]] | minor issues | [[Review Notes/Quantum Tricks/On-the-Fly Coherent Molecular Integral Computation - Review]] |
| [[Dynamic SELECT for Second-Quantized Hamiltonians]] | major issues | [[Review Notes/Quantum Tricks/Dynamic SELECT for Second-Quantized Hamiltonians - Review]] |
| [[Adiabatic State Preparation for Chemistry]] | major issues | [[Review Notes/Quantum Tricks/Adiabatic State Preparation for Chemistry - Review]] |
| [[Recursive Phase Estimation for Qubit-Efficient Readout]] | minor issues | [[Review Notes/Quantum Tricks/Recursive Phase Estimation for Qubit-Efficient Readout - Review]] |
| [[Givens Rotation Slater Determinant Preparation]] | minor issues | [[Review Notes/Quantum Tricks/Givens Rotation Slater Determinant Preparation - Review]] |
| [[Basis Switching via QFT for Kinetic-Potential Splitting]] | minor issues | [[Review Notes/Quantum Tricks/Basis Switching via QFT for Kinetic-Potential Splitting - Review]] |
| [[Parity-Counted Fermionic Sign Tracking]] | minor issues | [[Review Notes/Quantum Tricks/Parity-Counted Fermionic Sign Tracking - Review]] |
| [[Block-Diagonal Decomposition for Parallel Hamiltonian Simulation]] | minor issues | [[Review Notes/Quantum Tricks/Block-Diagonal Decomposition for Parallel Hamiltonian Simulation - Review]] |

## Tranche 8 Pattern Notes

- Early chemistry notes are strongest at the story level but often blur representation regimes: occupation-number/Jordan-Wigner mappings, first-quantized particle registers, real-space grids, plane-wave dual bases, and qubitized oracles should not be described with one generic "Pauli terms" vocabulary.
- Several resource statements need parameter hygiene. `N` may mean spin orbitals, spatial orbitals, plane-wave grid points, or particles; `eta`, `B`, `m`, volume, density, and precision assumptions must be named before quoting asymptotic scalings.
- The biggest technical problems are unitary-oracle details: SELECT must select Pauli/unitary branches, not ladder operators; adiabatic preparation evolves in physical time with a schedule; Fourier-eigenstate kickback avoids only the value-register uncompute, not arbitrary scratch cleanup.
- Trotter chemistry cards should distinguish per-step term cost, number of Trotter steps, commutator/error scale, and the physical input representation. A swap-network result for `T + U + V` density-density Hamiltonians is not automatically a result for generic four-index Gaussian chemistry.
- Historical notes should keep original-paper claims separate from later repairs: Abrams-Lloyd's antisymmetrization sketch versus sorting-network fixes, Aspuru-Guzik's direct mapping versus later JW/BK compilation, and Lanyon's precompiled H2 blocks versus scalable molecular simulation.

## Next Tranche

Recommended next cluster: fault-tolerant and qubitized quantum chemistry resource estimates: CI representation, linear T complexity, low-rank/double-factorization, sparse arbitrary-basis qubitization, tensor hypercontraction, sublinear basis scaling, and modern first-quantized chemistry.

## Completed Tranche 9 - Qubitized / Fault-Tolerant Chemistry Resource Estimates

| Note | Verdict | Review file |
|---|---:|---|
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) - Review]] |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) - Review]] |
| [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) - Review]] |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) - Review]] |
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) - Review]] |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) - Review]] |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) - Review]] |
| [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) - Review]] |
| [[QROM (Quantum Read-Only Memory)]] | major issues | [[Review Notes/Quantum Tricks/QROM (Quantum Read-Only Memory) - Review]] |
| [[QROAM (Space-Time Tradeoff for QROM)]] | major issues | [[Review Notes/Quantum Tricks/QROAM (Space-Time Tradeoff for QROM) - Review]] |
| [[Measurement-Based QROM Uncomputation]] | major issues | [[Review Notes/Quantum Tricks/Measurement-Based QROM Uncomputation - Review]] |
| [[Coherent Alias Sampling for PREPARE]] | major issues | [[Review Notes/Quantum Tricks/Coherent Alias Sampling for PREPARE - Review]] |
| [[Unary Iteration]] | minor issues | [[Review Notes/Quantum Tricks/Unary Iteration - Review]] |
| [[Single Factorization for Qubitized Chemistry]] | minor issues | [[Review Notes/Quantum Tricks/Single Factorization for Qubitized Chemistry - Review]] |
| [[Sparse State Preparation for Qubitized Chemistry]] | minor issues | [[Review Notes/Quantum Tricks/Sparse State Preparation for Qubitized Chemistry - Review]] |
| [[Double Factorization of Two-Electron Integrals]] | minor issues | [[Review Notes/Quantum Tricks/Double Factorization of Two-Electron Integrals - Review]] |
| [[THC Non-Orthogonal Diagonal Coulomb Representation]] | major issues | [[Review Notes/Quantum Tricks/THC Non-Orthogonal Diagonal Coulomb Representation - Review]] |
| [[QROM-Loaded Givens Rotation Networks]] | minor issues | [[Review Notes/Quantum Tricks/QROM-Loaded Givens Rotation Networks - Review]] |
| [[Kinetic Energy as Bit-Product LCU]] | minor issues | [[Review Notes/Quantum Tricks/Kinetic Energy as Bit-Product LCU - Review]] |
| [[Failed PREP Fallback to Alternative Hamiltonian Term]] | minor issues | [[Review Notes/Quantum Tricks/Failed PREP Fallback to Alternative Hamiltonian Term - Review]] |
| [[Qubitized Dyson Series for Phase Estimation]] | minor issues | [[Review Notes/Quantum Tricks/Qubitized Dyson Series for Phase Estimation - Review]] |
| [[Incremental Kinetic Energy Register]] | minor issues | [[Review Notes/Quantum Tricks/Incremental Kinetic Energy Register - Review]] |
| [[Interaction Picture Simulation (Kinetic Frame)]] | major issues | [[Review Notes/Quantum Tricks/Interaction Picture Simulation (Kinetic Frame) - Review]] |
| [[QROM Interpolation for Negative Exponential]] | minor issues | [[Review Notes/Quantum Tricks/QROM Interpolation for Negative Exponential - Review]] |
| [[Gramian-Based Momentum Norm for Non-Cubic Cells]] | minor issues | [[Review Notes/Quantum Tricks/Gramian-Based Momentum Norm for Non-Cubic Cells - Review]] |
| [[Arithmetic vs QROM for Pseudopotential Evaluation]] | minor issues | [[Review Notes/Quantum Tricks/Arithmetic vs QROM for Pseudopotential Evaluation - Review]] |
| [[Shared Exponential Evaluation Across Pseudopotential Terms]] | minor issues | [[Review Notes/Quantum Tricks/Shared Exponential Evaluation Across Pseudopotential Terms - Review]] |

## Tranche 9 Pattern Notes

- The strongest recurring error is confusing a clean story-level algorithm with the exact standard-form/block-encoding oracle. QROM garbage, alias-sampling garbage, measurement-based cleanup, dirty ancillae, and SELECT branches all have preconditions; they are not interchangeable "free uncompute" mechanisms.
- Several notes overstate asymptotic comparisons by dropping parameter conditions. In this cluster, `eta << N` is often not enough; comparisons may require `eta^2 << N`, `N >> eta^6`, or a fixed-density/grid-spacing convention.
- First-quantized chemistry notes need sharper separation between CI-orbital encodings, first-quantized plane-wave particle registers, and second-quantized plane-wave-dual bases. They share some vocabulary but have different `N`, `eta`, `lambda`, and qubit-count meanings.
- Factorization notes should distinguish empirical rank behavior from theorems. `L=O(N)`, THC rank `M=O(N)`, and lambda-ordering claims are benchmark-supported or construction-dependent, not universal facts for arbitrary chemistry Hamiltonians.
- Resource comparisons must name the metric: raw Toffolis/T gates, logical qubits, physical qubits, runtime, or spacetime volume. The same paper can lose on Toffolis and win on spacetime because of a large qubit-count gap.
- The pseudopotential cards are relatively strong. Their main issue is wording: arithmetic over QROM is exponential in the number of address bits, not a blanket complexity-theoretic exponential speedup, and it trades circuit cost against a large `lambda_nonloc`.

## Next Tranche

Recommended next cluster: post-2021 chemistry and materials follow-ups: Bloch-orbital materials, condensed-phase Trotterization, force/gradient estimation, stopping power, quantum fast multipole, improved initial-state preparation, and notes on whether chemistry targets actually yield quantum advantage.

## Completed Tranche 10 - Post-2021 Chemistry / Materials Follow-Ups

| Note | Verdict | Review file |
|---|---:|---|
| [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) - Review]] |
| [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) - Review]] |
| [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) - Review]] |
| [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) - Review]] |
| [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) - Review]] |
| [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) - Review]] |
| [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) - Review]] |
| [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) - Review]] |
| [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) - Review]] |
| [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) - Review]] |
| [[Faster Quantum Chemistry Simulations via Improved Tensor Factorization and Active Volume Compilation (Caesura, Cortes, Pol, Sim, Steudtner, Anselmetti, Degroote, Moll, Santagati, Streif, Tautermann 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Faster Quantum Chemistry Simulations via Improved Tensor Factorization and Active Volume Compilation (Caesura, Cortes, Pol, Sim, Steudtner, Anselmetti, Degroote, Moll, Santagati, Streif, Tautermann 2025) - Review]] |
| [[Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) - Review]] |
| [[k-Point THC Factorization (Bloch Orbital THC)]] | minor issues | [[Review Notes/Quantum Tricks/k-Point THC Factorization (Bloch Orbital THC) - Review]] |
| [[Controlled Momentum-Register Swaps for Periodic Block Encodings]] | minor issues | [[Review Notes/Quantum Tricks/Controlled Momentum-Register Swaps for Periodic Block Encodings - Review]] |
| [[Force Operator Block Encoding via Hamiltonian Differentiation]] | major issues | [[Review Notes/Quantum Tricks/Force Operator Block Encoding via Hamiltonian Differentiation - Review]] |
| [[FFFT Diagonalisation of Force Operators in Plane-Wave Basis]] | minor issues | [[Review Notes/Quantum Tricks/FFFT Diagonalisation of Force Operators in Plane-Wave Basis - Review]] |
| [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]] | minor issues | [[Review Notes/Quantum Tricks/EQA Assessment Framework (State Preparation vs Classical Heuristic) - Review]] |
| [[ASCI Overlap Estimation for QPE Initial State]] | minor issues | [[Review Notes/Quantum Tricks/ASCI Overlap Estimation for QPE Initial State - Review]] |
| [[Partial-Column Unitary Synthesis for MPS Preparation]] | minor issues | [[Review Notes/Quantum Tricks/Partial-Column Unitary Synthesis for MPS Preparation - Review]] |
| [[MPS Overlap Extrapolation Protocol]] | minor issues | [[Review Notes/Quantum Tricks/MPS Overlap Extrapolation Protocol - Review]] |
| [[Rectangular Block-Encoding for Spectrum Amplification]] | minor issues | [[Review Notes/Quantum Tricks/Rectangular Block-Encoding for Spectrum Amplification - Review]] |
| [[SOS Decomposition via Semidefinite Programming for Chemistry]] | minor issues | [[Review Notes/Quantum Tricks/SOS Decomposition via Semidefinite Programming for Chemistry - Review]] |
| [[DFTHC Factorization]] | minor issues | [[Review Notes/Quantum Tricks/DFTHC Factorization - Review]] |
| [[SOS Spectral Amplification]] | major issues | [[Review Notes/Quantum Tricks/SOS Spectral Amplification - Review]] |

## Tranche 10 Pattern Notes

- The main recurring chemistry/materials issue is comparison hygiene. Many notes correctly identify the winning idea but then compare unlike quantities: walk-step cost versus total QPE cost, Toffoli count versus runtime, or Hamiltonian factorization improvement versus hardware-layout speedup.
- Several 2025 notes are version-sensitive. In particular, the current QFMM arXiv v2 changes the crossover condition to `N < eta^7`; older `eta^6` wording should be dated or updated.
- Initial-state notes should distinguish determinant overlap, CSF overlap, MPS overlap, amplitude overlap, and squared overlap. Those numbers are not interchangeable in QPE/filtering cost formulas.
- Modern factorization names need sharper separation: RC-DF, SCDF, BLISS-THC, DFTHC, THC, and spectrum-amplified SOS methods are related but distinct. The vault should not import resource numbers across them without naming the exact algorithm and hardware assumptions.
- SOS/spectrum-amplification notes need to say what is mathematically abstract versus efficiently structured. Abstract PSD factorization is not the same as a low-cost chemistry SOS block encoding.
- Force-gradient cards should be careful with derivative algebra. Differentiating Hamiltonian data can introduce new oracle data and response terms; it is not always just "differentiate the coefficient tensor".

## Next Tranche

Recommended next cluster: remaining non-chemistry or cross-domain notes, beginning with recent quantum advantage/data-processing claims and then moving through any unreviewed algorithmic primitives outside the chemistry spine.

## Completed Tranche 11 - Quantum Advantage / Dequantization / Learning Readout

| Note | Verdict | Review file |
|---|---:|---|
| [[Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) - Review]] |
| [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) - Review]] |
| [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) - Review]] |
| [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) - Review]] |
| [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) - Review]] |
| [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) - Review]] |
| [[Learning quantum states prepared by shallow circuits in polynomial time (Landau-Liu 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Learning quantum states prepared by shallow circuits in polynomial time (Landau-Liu 2024) - Review]] |
| [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) - Review]] |
| [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) - Review]] |
| [[Classical shadows (Huang-Kueng-Preskill 2020)]] | major issues | [[Review Notes/Paper Summaries/Classical shadows (Huang-Kueng-Preskill 2020) - Review]] |
| [[Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) - Review]] |
| [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) - Review]] |
| [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) - Review]] |
| [[Quantum Oracle Sketching]] | minor issues | [[Review Notes/Quantum Tricks/Quantum Oracle Sketching - Review]] |
| [[Interferometric Classical Shadow]] | minor issues | [[Review Notes/Quantum Tricks/Interferometric Classical Shadow - Review]] |
| [[Quantum Uploading as One-Time Input Noise]] | minor issues | [[Review Notes/Quantum Tricks/Quantum Uploading as One-Time Input Noise - Review]] |
| [[Heisenberg Learning Tree Lower Bounds]] | minor issues | [[Review Notes/Quantum Tricks/Heisenberg Learning Tree Lower Bounds - Review]] |
| [[Permutation-Observable Noise Deconvolution for Multi-Copy Moments]] | minor issues | [[Review Notes/Quantum Tricks/Permutation-Observable Noise Deconvolution for Multi-Copy Moments - Review]] |
| [[Upload-Then-Randomize for Noisy Shadows]] | minor issues | [[Review Notes/Quantum Tricks/Upload-Then-Randomize for Noisy Shadows - Review]] |
| [[Spectral Density Estimation as Dequantization Shield]] | minor issues | [[Review Notes/Quantum Tricks/Spectral Density Estimation as Dequantization Shield - Review]] |
| [[DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues]] | minor issues | [[Review Notes/Quantum Tricks/DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues - Review]] |
| [[Average-Case vs Worst-Case QML Advantage Test]] | minor issues | [[Review Notes/Quantum Tricks/Average-Case vs Worst-Case QML Advantage Test - Review]] |
| [[Holevo Information Bottleneck for Learning Lower Bounds]] | minor issues | [[Review Notes/Quantum Tricks/Holevo Information Bottleneck for Learning Lower Bounds - Review]] |
| [[Bell Difference Sampling for Magnitude Estimation]] | minor issues | [[Review Notes/Quantum Tricks/Bell Difference Sampling for Magnitude Estimation - Review]] |
| [[Quantum Bohnenblust--Hille Inequality for Local Observables]] | minor issues | [[Review Notes/Quantum Tricks/Quantum Bohnenblust--Hille Inequality for Local Observables - Review]] |
| [[Thresholded Low-Degree Heisenberg Learning]] | minor issues | [[Review Notes/Quantum Tricks/Thresholded Low-Degree Heisenberg Learning - Review]] |
| [[Projected Quantum Kernel via Local Observables]] | major issues | [[Review Notes/Quantum Tricks/Projected Quantum Kernel via Local Observables - Review]] |
| [[Shadow Kernel for Nonlinear Phase Classification]] | minor issues | [[Review Notes/Quantum Tricks/Shadow Kernel for Nonlinear Phase Classification - Review]] |
| [[Quantum-Aided Compressed Sensing for Sparse State Readout]] | minor issues | [[Review Notes/Quantum Tricks/Quantum-Aided Compressed Sensing for Sparse State Readout - Review]] |

## Tranche 11 Pattern Notes

- Advantage claims in this cluster need a resource label every time: query, sample, memory/space, runtime, wall-clock data streaming, classical post-processing, or quantum copies. Many notes are good, but the dangerous summaries are the ones that say "exponential advantage" without the metric.
- The 2026 notes are current arXiv v1 papers. Their exact constants and lower-bound formulas should be treated as version-sensitive, while the model-level ideas can be reviewed normally.
- Learning and shadow notes repeatedly need the same distinction: average-case prediction, worst-case property prediction, and full tomography are different problems.
- Topological-data-analysis notes should keep three regimes separate: exact Betti computation, normalized approximate Betti/spectral-density estimation, and clique-complex-specific algorithms. DQC1-hardness for a generalized spectral problem does not automatically prove practical TDA advantage.
- Classical-shadow cards should never state `O(log M / epsilon^2)` without the shadow norm, observable family, and measurement ensemble.
- Several source notes have literal equation-corruption characters where `\bigl` or `\frac` should appear. Those should be fixed before content revisions because they make the notes unreliable in Obsidian rendering and search.

## Next Tranche

Recommended next cluster: arithmetic, QFT, adders, and fault-tolerant compilation primitives, especially the 2025 Gidney/Kahanamoku-Meyer notes and older Draper/Haner/Roetteler arithmetic summaries.

## Completed Tranche 12 - Arithmetic / QFT / Adder Primitives

| Note | Verdict | Review file |
|---|---:|---|
| [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) - Review]] |
| [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) - Review]] |
| [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Addition on a Quantum Computer (Draper 2000) - Review]] |
| [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) - Review]] |
| [[Arithmetic in the Fourier Basis]] | minor issues | [[Review Notes/Quantum Tricks/Arithmetic in the Fourier Basis - Review]] |
| [[Quantum Fourier Transform Circuit]] | major issues | [[Review Notes/Quantum Tricks/Quantum Fourier Transform Circuit - Review]] |
| [[Optimistic Quantum Circuits]] | major issues | [[Review Notes/Quantum Tricks/Optimistic Quantum Circuits - Review]] |
| [[Quantum Phase Estimation on Blocks for QFT Parallelisation]] | minor issues | [[Review Notes/Quantum Tricks/Quantum Phase Estimation on Blocks for QFT Parallelisation - Review]] |
| [[Worst-to-Average-Case Reduction for Optimistic Circuits]] | minor issues | [[Review Notes/Quantum Tricks/Worst-to-Average-Case Reduction for Optimistic Circuits - Review]] |
| [[Carry Venting via X-Basis Measurement]] | minor issues | [[Review Notes/Quantum Tricks/Carry Venting via X-Basis Measurement - Review]] |
| [[Half-Register Dirty Workspace Borrowing]] | minor issues | [[Review Notes/Quantum Tricks/Half-Register Dirty Workspace Borrowing - Review]] |
| [[Carry-Lookahead Tree for Logarithmic-Depth Addition]] | minor issues | [[Review Notes/Quantum Tricks/Carry-Lookahead Tree for Logarithmic-Depth Addition - Review]] |
| [[Phase Overlapping in Tree Circuits]] | minor issues | [[Review Notes/Quantum Tricks/Phase Overlapping in Tree Circuits - Review]] |
| [[MAJ-UMA Decomposition for Reversible Adders]] | minor issues | [[Review Notes/Quantum Tricks/MAJ-UMA Decomposition for Reversible Adders - Review]] |
| [[Two's-Complement Carry Erasure for In-Place Adders]] | minor issues | [[Review Notes/Quantum Tricks/Two's-Complement Carry Erasure for In-Place Adders - Review]] |

## Tranche 12 Pattern Notes

- Arithmetic notes must keep gate count, circuit depth, T/Toffoli count, rotation synthesis cost, ancilla count, and connectivity separate. Several comparisons become wrong when "abstract controlled rotation" is treated like a fault-tolerant primitive.
- The 2025 optimistic-QFT notes repeatedly overstate locality as nearest-neighbor. The source result is logarithmic gate range/locality in a 1D layout for the optimistic circuit; the reductions may require long-range additions.
- QFT-addition notes should distinguish the commuting Fourier-basis addition phase from the full QFT-add-inverse-QFT adder. Parallelizing the middle phase does not by itself make the whole adder log depth.
- Carry-based adders split cleanly into two design families: ripple/MAJ-UMA minimizes workspace and T count at linear depth; carry-lookahead minimizes depth at `O(n)` ancilla cost.
- Measurement-based arithmetic tricks such as carry venting are not free uncomputation. They trade quantum garbage for classical measurement records plus later phase-correction circuits.

## Next Tranche

Recommended next cluster: modular multiplication, Shor resource-estimate arithmetic, Karatsuba/reversible multiplication, approximate encoded permutations, and Clifford+T synthesis/rotation-compilation notes.

## Completed Tranche 13 - Modular Arithmetic / Multiplication / Clifford+T Synthesis

| Note | Verdict | Review file |
|---|---:|---|
| [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) - Review]] |
| [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) - Review]] |
| [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) - Review]] |
| [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) - Review]] |
| [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) - Review]] |
| [[Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014) - Review]] |
| [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) - Review]] |
| [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) - Review]] |

## Tranche 13 Pattern Notes

- Arithmetic resource estimates need exact metric labels. Toffoli count, T count, T depth, measurement depth, surface-code spacetime volume, and physical runtime are different quantities.
- The modular-arithmetic notes are generally good, but later-resource comparisons are easy to misquote. Gidney-Ekerå-style formulas should be recomputed from the cited polynomial rather than carried as rounded folklore.
- Recursive arithmetic notes need algebraic sanity checks. A displayed exponent formula that does not evaluate to the claimed exponent is a serious issue even if the surrounding prose is right.
- RUS and measurement-assisted synthesis notes must report expected cost, variance/tails, and worst-case retry policy separately. "Expected T-count" is not a complete fault-tolerant resource estimate.
- Comparator-based state preparation removes arcsine arithmetic, but it does not remove oracle-loading cost or amplitude-amplification dependence on success probability.

## Next Tranche

Recommended next cluster: Shor/order-finding/factoring-family papers, including primality/order variants, safe semiprime factoring, deterministic classical factoring comparisons, elliptic-curve/isogeny notes, and cryptanalytic resource estimates.

## Completed Tranche 14 - Factoring / Primality / Crypto / Hidden Shift

| Note | Verdict | Review file |
|---|---:|---|
| [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) - Review]] |
| [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) - Review]] |
| [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) - Review]] |
| [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) - Review]] |
| [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) - Review]] |
| [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) - Review]] |
| [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) - Review]] |
| [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) - Review]] |
| [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) - Review]] |
| [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) - Review]] |
| [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) - Review]] |
| [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Computation and Lattice Problems (Regev 2002) - Review]] |
| [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) - Review]] |
| [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) - Review]] |
| [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) - Review]] |
| [[Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) - Review]] |

## Tranche 14 Pattern Notes

- Certificate statements need a verifier model. A base with order `N-1` is a quantum-checkable primality certificate unless one also supplies enough classical data about `N-1`.
- Shor-wrapper improvements should state the measured resource. One QOFA call, one modular-exponentiation circuit, logical gate count, and fault-tolerant area-time are not interchangeable.
- Classical number-theory comparisons must keep theorem/proof status separate from heuristic prover time. ECPP-style certificates can be deterministically checked even when expected generation time is heuristic.
- Grover cryptanalysis notes need separate query count, reversible-oracle logical cost, T-depth/T-width, factory throughput, and area-time cost. "Grover halves security" is not a resource estimate.
- Hidden-shift/HSP notes need exact space labels. Kuperberg 2005, Regev 2004, and Kuperberg 2013 trade time, quantum memory, classical memory, and QRACM in different ways.
- Lattice notes must distinguish hidden-subgroup reductions for unique-SVP from Groverized sieving for exact SVP. Neither is a polynomial-time break of general lattice cryptography.

## Next Tranche

Recommended next cluster: remaining HSP and nonabelian-algebra papers, including semidirect-product HSP, optimal measurements/PGM, quantum query complexity of HSP, Pell/principal ideal algorithms, and nearby quantum walk/query-model papers.

## Completed Tranche 15 - HSP / Nonabelian Algebra / Algebraic Number Theory

| Note | Verdict | Review file |
|---|---:|---|
| [[The Hidden Subgroup Problem Review and Open Problems (Lomont 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Hidden Subgroup Problem Review and Open Problems (Lomont 2004) - Review]] |
| [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) - Review]] |
| [[On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) - Review]] |
| [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) - Review]] |
| [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) - Review]] |
| [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) - Review]] |
| [[Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009) - Review]] |
| [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Solvable Groups (Watrous 2001) - Review]] |
| [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) - Review]] |
| [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) - Review]] |
| [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) - Review]] |
| [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) - Review]] |
| [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) - Review]] |
| [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) - Review]] |
| [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) - Review]] |
| [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008) - Review]] |
| [[Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016) - Review]] |
| [[A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013) - Review]] |

## Tranche 15 Pattern Notes

- Do not conflate query complexity with efficient computation. EHK-style results show that enough coset-state information exists; they do not give efficient measurements or post-processing.
- Nonabelian HSP summaries need exact family boundaries. "Solvable group algorithms", "normal-HSP algorithms", and "HSP over all solvable groups" are different claims.
- PGM/optimal-measurement notes should separate optimality of a measurement from efficient implementation of that measurement. Matrix-sum or basis-selection subproblems are often the bottleneck.
- Continuous-period and infrastructure algorithms need precise problem scope. Hallgren 2002 is Pell/regulator/PIP for real quadratic fields; later class-group/unit-group algorithms should be attributed separately.
- Semidirect-product papers in this cluster are mostly subgroup-classification plus abelian reductions. They do not solve dihedral, quasi-dihedral, affine, or symmetric-group HSP in general.
- Several source notes from Algorithm Zoo imports lack top-level headings; add headings before future vault-wide publishing.

## Next Tranche

Recommended next cluster: quantum walk and query-complexity papers, including glued trees, element distinctness, NAND/formula evaluation, adversary/polynomial method notes, collision/lower-bound papers, and related walk-search algorithms.

## Completed Tranche 16 - Query Complexity / Lower Bounds / Search Variants

| Note | Verdict | Review file |
|---|---:|---|
| [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) - Review]] |
| [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) - Review]] |
| [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) - Review]] |
| [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) - Review]] |
| [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) - Review]] |
| [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Search with Advice (Montanaro 2009) - Review]] |
| [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) - Review]] |
| [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) - Review]] |
| [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) - Review]] |
| [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) - Review]] |
| [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) - Review]] |
| [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) - Review]] |
| [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) - Review]] |
| [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) - Review]] |
| [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) - Review]] |
| [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) - Review]] |
| [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) - Review]] |

## Tranche 16 Pattern Notes

- Query-model notes need exact model labels: total versus partial functions, bounded-error versus exact/zero-error, and combined-oracle versus separate-oracle query accounting.
- Positive adversary, weighted positive adversary, and negative/general adversary should not be collapsed into one method. The certificate-complexity barrier applies to the positive-weight world, not to Reichardt-style tight general adversary.
- Classical comparison claims for approximate statistics need their access model. Approximate median can look very different in comparison-tree, value-oracle, randomized sampling, and worst-case deterministic models.
- Numerical constants in ordered search are delicate. Do not call a paper-specific quantity "bits of information" if it exceeds the entropy of the measurement outcome.
- Convex-optimization summaries need a full rewrite if the source paper is Chakrabarti-Childs-Li-Wu 2020: its core is `\tilde O(n)` evaluation/membership-query optimization over convex bodies, not QLSA-based IPM or gradient-query subgradient methods.
- Several Algorithm-Zoo-style source notes still lack a top-level heading.

## Next Tranche

Recommended next cluster: simulation, differential-equation, linear-system, QSP/QSVT, and Hamiltonian-algorithm papers that are still unreviewed.

## Completed Tranche 17 - QLSP / Differential Equations / Spectral PDEs

| Note | Verdict | Review file |
|---|---:|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) - Review]] |
| [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) - Review]] |
| [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) - Review]] |
| [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) - Review]] |
| [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) - Review]] |
| [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) - Review]] |
| [[Quantum Linear Matrix Equations — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Linear Matrix Equations - Review]] |
| [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) - Review]] |
| [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) - Review]] |
| [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] | major omissions | [[Review Notes/Paper Summaries/Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) - Review]] |
| [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) - Review]] |
| [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) - Review]] |

## Tranche 17 Pattern Notes

- Linear-system notes must separate state preparation from observable estimation and sampling/output tasks. Lower bounds that rule out polylogarithmic dependence for one output model do not automatically rule out improved dependence for preparing the solution state.
- QLSP complexity statements need the access model: sparse-access, block-encoding, Hamiltonian simulation, QRAM/data structure, and matrix-entry oracle assumptions are not interchangeable.
- BQP-completeness statements should name the promise problem and scaling regime. Vague claims that a QLSP solver is "BQP-complete" hide the role of condition number, precision, normalization, and the requested output.
- Adiabatic and randomized QLSP algorithms are sensitive to constants, but numerical comparisons on small random matrices should be framed as empirical evidence, not as dominance theorems.
- Differential-equation summaries need to distinguish time-independent linear ODEs, time-dependent linear ODEs, nonlinear dissipative systems, PDE discretizations, and spectral methods. Papers in this area are easy to misfile as generic "ODE solvers."
- Spectral-method and PDE notes should state quantitative smoothness/derivative assumptions. `C^\infty` by itself does not imply the super-exponential or exponential convergence bounds used in the algorithms.

## Next Tranche

Recommended next cluster: remaining differential-equation, PDE, non-unitary-dynamics, and eigenvalue-transform papers, then move into the still-unreviewed QSP/QSVT and Hamiltonian-simulation papers.

## Completed Tranche 18 - Differential Equations / Non-Unitary Dynamics / Eigenvalue Transforms

| Note | Verdict | Review file |
|---|---:|---|
| [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) - Review]] |
| [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) - Review]] |
| [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) - Review]] |
| [[Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) - Review]] |
| [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) - Review]] |
| [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) - Review]] |
| [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) - Review]] |
| [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) - Review]] |
| [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) - Review]] |
| [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) - Review]] |
| [[Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) - Review]] |
| [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) - Review]] |
| [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) - Review]] |
| [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) - Review]] |
| [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) - Review]] |
| [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) - Review]] |
| [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) - Review]] |
| [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) - Review]] |

## Tranche 18 Pattern Notes

- PDE and ODE speedup claims need the output model first. Producing a normalized quantum state, estimating a scalar functional, estimating a matrix entry, and printing a full solution vector are different problems.
- Differential-equation notes should keep stability parameters distinct: condition number, amplification ratio, log norm, `C(A)`, Carleman `R`, and output-norm ratios are not interchangeable.
- Nonlinear-DE summaries should avoid saying "BQP-hard" when the result is an oracle/query lower bound against efficient quantum simulation in a parameter regime.
- LCHS/Lap-LCHS/QEVT notes are especially sensitive to kernel constants, half-plane conventions, and transform-domain parameters. Small sign or normalization errors can reverse the conditioning story.
- Recent and arXiv-v1 notes need maintenance markers. Bibliographic data, theorem numbering, and even titles may change quickly.
- Several notes use "References within this paper" for later cross-links. Split those sections into actual in-paper references and vault cross-links before publishing.

## Next Tranche

Recommended next cluster: QSP/QSVT, Hamiltonian simulation, product-formula, qDRIFT, LCU, and qubitization papers that remain unreviewed.

## Completed Tranche 19 - QSP / QSVT / Hamiltonian Simulation Foundations

| Note | Verdict | Review file |
|---|---:|---|
| [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/LCU Origins (Childs-Wiebe 2012) - Review]] |
| [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) - Review]] |
| [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) - Review]] |
| [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) - Review]] |
| [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) - Review]] |
| [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) - Review]] |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] | minor omissions | [[Review Notes/Paper Summaries/Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) - Review]] |
| [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Hamiltonian Simulation by Qubitization (Low-Chuang 2019) - Review]] |
| [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) - Review]] |
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] | minor omissions | [[Review Notes/Paper Summaries/QSVT and Beyond (Gilyén et al. 2018-2019) - Review]] |
| [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) - Review]] |
| [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) - Review]] |
| [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) - Review]] |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) - Review]] |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) - Review]] |
| [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) - Review]] |
| [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) - Review]] |
| [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) - Review]] |

## Tranche 19 Pattern Notes

- Hamiltonian-simulation summaries must keep query complexity, gate complexity, and normalization conventions separate. `t`, `\tau`, `\alpha t`, sparsity-normalized time, and max-norm-normalized time are not interchangeable.
- The post-2015 simulation lineage is often misstated. Truncated Taylor gives multiplicative `\tau log(\tau/\epsilon)/loglog(\tau/\epsilon)` behavior, while Low-Chuang QSP/qubitization gives the additive optimal form `\tau + log(1/\epsilon)/loglog(1/\epsilon)`.
- Qubitization notes need the block-encoding/select-prepare parameter `\alpha` up front. Claims written only as `O(t + log(1/\epsilon))` hide the key resource.
- Uniform spectral amplification is easy to misread because the advertised speedup is a normalization amplification statement, not a free low-energy `sqrt(\Delta)` time scaling in all regimes.
- LCU/Taylor/Dyson notes should distinguish exact oblivious amplitude amplification, robust variants, truncation error, and discretization error. These are different error sources.
- Product-formula comparison tables need careful historical attribution: Childs-Wiebe 2012 is LCU/multiproduct, BCCKS 2015 is truncated Taylor, Low-Chuang is QSP/qubitization, and qDRIFT/randomized formulas are a separate branch.

## Next Tranche

Recommended next cluster: product-formula, randomized-product-formula, lattice-Hamiltonian, corrected-product-formula, and recent Hamiltonian-simulation implementation papers.

## Completed Tranche 20 - Remaining Product-Formula / Lattice / Randomized Simulation Papers

| Note | Verdict | Review file |
|---|---:|---|
| [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) - Review]] |
| [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) - Review]] |
| [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] | minor omissions | [[Review Notes/Paper Summaries/Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) - Review]] |
| [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]] | major omissions | [[Review Notes/Paper Summaries/Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) - Review]] |
| [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) - Review]] |
| [[Shadow Hamiltonian Simulation — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Shadow Hamiltonian Simulation - Review]] |
| [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) - Review]] |
| [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) - Review]] |
| [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) - Review]] |

## Tranche 20 Pattern Notes

- State-dependent simulation guarantees need precise quantifiers: worst-case operator norm, random-input average error, fixed-vector norm, local-observable error, and expectation-value-only estimators are different products.
- Product-formula refinements should name their primitive set. THRIFT, corrected product formulas, multiproduct sampling, qFLO, and symmetry protection are not interchangeable even when they all "reduce Trotter error."
- Recent 2025 notes need bibliographic checking. Article numbers, journal status, author lists, and arXiv versions have already drifted in a few summaries.
- Lattice-simulation gate complexity should separate query complexity from gate-level implementation of sparse/block oracles. HHKL is mainly a gate-complexity/geometric-locality result.
- Average-case or randomized methods usually output expectation estimates or channels, not coherent unitary state-preparation subroutines. This must be visible before comparing them with QSP/LCU.

## Next Tranche

Recommended next cluster: remaining quantum chemistry, first-quantized/real-space/materials simulation, and Hamiltonian-learning papers that are still unreviewed.

## Completed Tranche 21 - Chemistry Experiments / UCC / Real-Space and Materials Simulation

| Note | Verdict | Review file |
|---|---:|---|
| [[Quantum Implementation of Unitary Coupled Cluster for Simulating Molecular Electronic Structure (Shen, Zhang, Chen, Zhang, Yung, Kim 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Implementation of Unitary Coupled Cluster for Simulating Molecular Electronic Structure (Shen, Zhang, Chen, Zhang, Yung, Kim 2015) - Review]] |
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) - Review]] |
| [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]] | minor caveats | [[Review Notes/Paper Summaries/Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) - Review]] |
| [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) - Review]] |
| [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) - Review]] |
| [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) - Review]] |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) - Review]] |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) - Review]] |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) - Review]] |
| [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) - Review]] |
| [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) - Review]] |
| [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) - Review]] |

## Tranche 21 Pattern Notes

- Early chemistry demonstrations need their accuracy claims tied to the encoded model and basis. Chemical accuracy, high-bit eigenvalue precision, and NMR/NV proof-of-principle precision do not by themselves imply scalable chemistry accuracy.
- "Scalable" is overloaded in these notes. Some papers prove polynomial fermion-to-qubit representation or compilation paths, while the reported experiment may still be a tiny fixed instance.
- UCC summaries should separate toy exact exponentials from general UCCSD. One-gate exactness for a one-parameter demonstrator is not evidence that full UCCSD avoids Trotter/product-formula structure.
- Representation papers need error layers kept separate: basis/discretization error, Hamiltonian-simulation error, variational/measurement error, and observable-estimation or reconstruction cost.
- First-quantized dynamics summaries need precise meanings for `N`, particle count, grid spacing, derivative bounds, output model, and measurement/postprocessing cost before comparing against classical dynamics.

## Next Tranche

Recommended next cluster: remaining quantum chemistry, fermionic measurement/operator-compression papers, Hamiltonian-learning papers, and specialized Hamiltonian-simulation applications.

## Completed Tranche 22 - VQE / Fermionic Measurement / Chemistry-Adjacent Simulation

| Note | Verdict | Review file |
|---|---:|---|
| [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) - Review]] |
| [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) - Review]] |
| [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) - Review]] |
| [[Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009) — Paper Notes]] | major issue | [[Review Notes/Paper Summaries/Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009) - Review]] |
| [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) - Review]] |
| [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]] | major omissions | [[Review Notes/Paper Summaries/Fermionic Eigenstate Prep Techniques (Nature 2018) - Review]] |
| [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) - Review]] |
| [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) - Review]] |
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) - Review]] |
| [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) - Review]] |
| [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes]] | major historical issue | [[Review Notes/Paper Summaries/Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) - Review]] |
| [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes]] | major optimizer-cost issue | [[Review Notes/Paper Summaries/Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) - Review]] |

## Tranche 22 Pattern Notes

- VQE summaries should separate ansatz expressibility, optimizer convergence, measurement shot cost, and hardware noise. "Short circuits" do not imply low total cost.
- Measurement papers need three resource columns: number of settings, gates per setting, and variance/shot count. Improvements in one column do not automatically improve the others.
- Fermionic and chemistry demonstrations need active-space/model qualifications near the accuracy claims, not only in limitations.
- Complexity and DFT notes should keep worst-case inverse-polynomial precision distinct from practical approximate chemistry.
- Experimental NISQ papers should be described as benchmarks unless they actually enter a classically hard regime.

## Completed Tranche 23 - Quantum Subspace Methods and Hamiltonian Learning

| Note | Verdict | Review file |
|---|---:|---|
| [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) - Review]] |
| [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) - Review]] |
| [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes]] | major omissions | [[Review Notes/Paper Summaries/Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) - Review]] |
| [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) - Review]] |
| [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) - Review]] |
| [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes]] | major omissions | [[Review Notes/Paper Summaries/Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) - Review]] |
| [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) - Review]] |
| [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) - Review]] |

## Tranche 23 Pattern Notes

- Hamiltonian-learning notes need an access-model box: known model family or sparse dictionary, trusted simulator, coherent interface or classical communication, Bayesian prior, and particle-filter assumptions.
- Bayesian "certification" should be described as model-dependent credible-region certification, not device-independent validation.
- Empirical `O(d log(1/epsilon))` convergence and RFPE Heisenberg scaling should not be promoted to full end-to-end theorems unless particle count, likelihood sampling, and noise assumptions are included.
- QSE/QSD notes need denominator and threshold caveats. Projection and generalized-eigenvalue methods work only while normalization, overlap-matrix conditioning, and measurement perturbations are controlled.
- Stub notes should stay visibly marked as incomplete in the review index until they contain the algorithm, guarantees, and limitations.

## Next Tranche

Recommended next cluster: QMA/local-Hamiltonian complexity, adiabatic computation, QAOA/optimization, and remaining ground-state preparation papers.

## Completed Tranche 24 - Local Hamiltonian Complexity / Adiabatic Computation / QAOA

| Note | Verdict | Review file |
|---|---:|---|
| [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]] | minor omissions | [[Review Notes/Paper Summaries/Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) - Review]] |
| [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]] | major omissions | [[Review Notes/Paper Summaries/3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) - Review]] |
| [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]] | minor omissions | [[Review Notes/Paper Summaries/2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) - Review]] |
| [[QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007) - Review]] |
| [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) - Review]] |
| [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) - Review]] |
| [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) - Review]] |
| [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) - Review]] |
| [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) - Review]] |
| [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) - Review]] |
| [[Robustness of Adiabatic Quantum Computation (Childs-Farhi-Preskill 2001) — Paper Notes]] | major scaling issues | [[Review Notes/Paper Summaries/Robustness of Adiabatic Quantum Computation (Childs-Farhi-Preskill 2001) - Review]] |
| [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) - Review]] |
| [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) - Review]] |
| [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) - Review]] |
| [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) - Review]] |
| [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) - Review]] |

## Tranche 24 Pattern Notes

- Complexity summaries need theorem settings up front: locality, local dimension, coupling signs, promise gap, free local terms, lattice restrictions, and whether the result is worst-case or typical-case.
- Adiabatic notes should distinguish model equivalence, actual optimization performance, and schedule design. Polynomial equivalence to circuits does not imply a useful adiabatic path for a given optimization instance.
- Gap language needs care. A minimum gap, an avoided crossing, a closed gap, and a rigorous lower bound are different statements.
- Open-system adiabatic robustness requires bath assumptions. A gap is helpful, but not a universal error-correction mechanism.
- QAOA notes should separate original-paper claims from later literature surveys, and gate count from parallel circuit depth.

## Next Tranche

Recommended next cluster: ground-state preparation and energy-estimation algorithms, quantum optimization descendants, and remaining Hamiltonian-simulation application papers.

## Completed Tranche 25 - Ground-State Preparation and Energy Estimation

| Note | Verdict | Review file |
|---|---:|---|
| [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Near-Optimal Ground State Preparation (Lin-Tong 2020) - Review]] |
| [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) - Review]] |
| [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) - Review]] |
| [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) - Review]] |

## Tranche 25 Pattern Notes

- Ground-state preparation notes must keep overlap amplitude and overlap probability separate. Several papers use `gamma` as an amplitude while Lin-Tong 2022 uses `eta` as a probability lower bound.
- "Heisenberg-limited" should state the resource being limited: total evolution time, maximum coherent evolution time, or query complexity. These are not interchangeable.
- Filtering thresholds need promise windows. A sign, step, or CDF test only works cleanly away from its transition region.
- QET-U and ACDF summaries should separate block-encoding, controlled Hamiltonian evolution, and control-free special implementations. These are different input models with different hardware implications.
- Chebyshev-filter formulas are easy to miscopy. The polynomial must be checked against the interval on which suppression is claimed; if it equals 1 at the edge of the unwanted spectrum, it is not the desired filter.

## Next Tranche

Recommended next cluster: Lindbladian and thermal-state preparation, spectral-gap amplification, quantum Metropolis, and recent low-energy-state heuristics.

## Completed Tranche 26 - Lindbladian / Thermal / Low-Energy State Preparation

| Note | Verdict | Review file |
|---|---:|---|
| [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]] | major issue | [[Review Notes/Paper Summaries/Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) - Review]] |
| [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Spectral Gap Amplification (Somma-Boixo 2013) - Review]] |
| [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) - Review]] |
| [[A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020) - Review]] |
| [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) - Review]] |

## Tranche 26 Pattern Notes

- State-preparation summaries need a status label: theorem, heuristic, local convergence result, or numerical evidence. These are often mixed in prose.
- For dissipative and Metropolis-type algorithms, the fixed point and the mixing time are separate facts. Correct stationary distribution does not imply efficient preparation.
- Entropy/free-energy variational algorithms should expose the entropy-estimation resource model. "Variational" does not automatically mean shallow Pauli measurements.
- Gap-amplification claims require normalization. Absolute gap, normalized gap, operator norm, and simulation cost must be compared together.
- In heuristic low-energy-state papers, high variance is only a proxy for possible descent; it is not the same as guaranteed spectral weight below the current energy.

## Next Tranche

Recommended next cluster: optimization descendants of QAOA, decoded/interference optimization methods, and remaining quantum gradient/heuristic compilation papers.

## Completed Tranche 27 - Quantum Optimization Descendants

| Note | Verdict | Review file |
|---|---:|---|
| [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) - Review]] |
| [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) - Review]] |
| [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) - Review]] |
| [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) - Review]] |
| [[Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005) - Review]] |

## Tranche 27 Pattern Notes

- Quantum-optimization notes should separate per-step primitive cost, number of heuristic steps, and full time-to-solution. These are often conflated.
- "Known classical algorithms" is not a hardness proof. Keep known-algorithm speedups separate from oracle separations and unconditional lower bounds.
- QAOA/SAT benchmarking should report median time-to-solution exponents separately from average success probabilities.
- Coherent oracle-model gradient improvements are not automatically NISQ measurement savings.
- Resource-estimation papers age quickly. Preserve the hardware model, year, and compiler assumptions beside any concrete Toffoli/qubit/time number.

## Next Tranche

Recommended next cluster: planted inference, quantum advantage/reality-check papers, and remaining Babbush-group optimization/simulation advantage papers.

## Completed Tranche 28 - Quantum Advantage Case Studies

| Note | Verdict | Review file |
|---|---:|---|
| [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) - Review]] |
| [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) - Review]] |
| [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) - Review]] |
| [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) - Review]] |
| [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) - Review]] |
| [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) - Review]] |

## Tranche 28 Pattern Notes

- Advantage claims need labels: unconditional lower bound, oracle/query lower bound, BQP-completeness, DQC1-hardness, best-known-classical comparison, empirical evidence, or resource estimate.
- "Natural problem" and "natural lower-bound instance" are different claims. Physical motivation does not make the hard instance physically typical.
- TDA notation is fragile. Track vertices/simplex dimension/homology dimension and Dirac-vs-Laplacian gaps explicitly.
- Resource estimates should carry their compiler, hardware model, and date.
- Framework-relative speedups, such as over Kikuchi or Prange-style attacks, should not be written as lower bounds against all classical algorithms.

## Next Tranche

Recommended next cluster: remaining speedup and advantage notes, including quantum walks, Monte Carlo/backtracking, data-processing advantage, and 2026 experiment-processing papers.

## Completed Tranche 29 - Speedup and Advantage Remainders

| Note | Verdict | Review file |
|---|---:|---|
| [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) - Review]] |
| [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) - Review]] |
| [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Speedup of Monte Carlo Methods (Montanaro 2015) - Review]] |
| [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) - Review]] |
| [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) - Review]] |
| [[Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) - Review]] |
| [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) - Review]] |
| [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) - Review]] |
| [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]] | major factual issues | [[Review Notes/Paper Summaries/Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) - Review]] |
| [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]] | duplicate / metadata issues | [[Review Notes/Paper Summaries/Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) - Review]] |

## Tranche 29 Pattern Notes

- "Exponential speedup" should always carry the comparison model: oracle/query, bounded-space streaming, raw noisy quantum processing, or best-known classical algorithms.
- Duplicate summaries should be marked as redirects. Otherwise later readers may cite the weaker duplicate and miss the caveats in the detailed note.
- Resource-estimation notes are especially prone to paper conflation. Keep the target Hamiltonian/problem family, compiler model, and year attached to every concrete number.
- Quantum-walk speedups need their oracle model and output task stated: finding an exit vertex, testing a property, detecting a marked backtracking node, or estimating an average are distinct tasks.
- For 2026 advantage papers, add a version/status line. These are fresh preprints and should not be written as settled textbook facts.

## Next Tranche

Recommended next cluster: remaining foundational algorithm primitives and communication/query complexity notes, including minimum finding, counting/amplitude estimation, collision/element distinctness, formula evaluation, fingerprinting, and quantum matrix verification.

## Completed Tranche 30 - Foundational Query and Communication Primitives

| Note | Verdict | Review file |
|---|---:|---|
| [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) - Review]] |
| [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Quantum Counting (Brassard-Høyer-Tapp 1998) - Review]] |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) - Review]] |
| [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) - Review]] |
| [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) - Review]] |
| [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) - Review]] |
| [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) - Review]] |
| [[Quantum Verification of Matrix Products (Buhrman-Špalek 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Verification of Matrix Products (Buhrman-Špalek 2005) - Review]] |
| [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) - Review]] |
| [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) - Review]] |
| [[Exponential Separation of Quantum and Classical Communication Complexity (Raz 1999) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Exponential Separation of Quantum and Classical Communication Complexity (Raz 1999) - Review]] |
| [[Consequences and Limits of Nonlocal Strategies (Cleve-Hoyer-Toner-Watrous 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Consequences and Limits of Nonlocal Strategies (Cleve-Hoyer-Toner-Watrous 2004) - Review]] |
| [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) - Review]] |
| [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Pattern Matching Fast on Average (Montanaro 2015) - Review]] |
| [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)]] | major omissions | [[Review Notes/Paper Summaries/Recursive Amplitude Amplification (Høyer-Mosca-de Wolf) - Review]] |

## Tranche 30 Pattern Notes

- Amplitude-amplification notes must separate ordinary integer Grover iteration, exact amplification with phase adjustment, amplitude estimation, and bounded-error input recursion.
- Exact-counting formulas need endpoint-safe statements. Any expression that vanishes at `t=0` or `t=N` is missing a special case or `+1` convention.
- Communication-complexity notes need a notation key. Exact, zero-error, bounded-error, quantum, randomized, public-coin, private-coin, SMP, one-way, and two-way models should not be mixed in one symbol.
- Collision and element-distinctness notes must keep the promise visible. `N^{1/3}` is for structured/r-to-one collision finding; arbitrary element distinctness is `N^{2/3}` in the optimal walk algorithm.
- Query slogans such as "one query for the gradient" should carry smoothness, scaling, precision, and coherent-oracle assumptions.

## Next Tranche

Recommended next cluster: quantum cryptanalysis, finite-field/discrete-log variants, lattice and representation-theoretic algorithms, plus Schur-transform notes.

## Completed Tranche 31 - Cryptanalysis, Finite Fields, Lattices, and Representation Transforms

| Note | Verdict | Review file |
|---|---:|---|
| [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) - Review]] |
| [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) - Review]] |
| [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Primality Test Via Quantum Factorization (Chau-Lo 1996) - Review]] |
| [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) - Review]] |
| [[An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) - Review]] |
| [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) - Review]] |
| [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) - Review]] |
| [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) - Review]] |
| [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) - Review]] |
| [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) - Review]] |
| [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) - Review]] |
| [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) - Review]] |
| [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) - Review]] |
| [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) - Review]] |
| [[Quantum Hermite Transform — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Quantum Hermite Transform - Review]] |

## Tranche 31 Pattern Notes

- Cryptanalysis notes need the adversary model next to every headline: ideal-cipher queries, Q1 data plus quantum processing, or Q2 coherent oracle access.
- Algebraic cryptanalysis via HHL should be labeled as condition-number and sparse-access dependent. Small polynomial exponents are meaningless without the `kappa` and oracle assumptions.
- Lattice notes must state the lattice class and parameter regime before the approximation slogan. A small BDD radius is an easier promise, not the same wording as a large approximate-SVP factor.
- Representation-theoretic algorithms should separate exact-by-rounding sampling, additive matrix-element estimation, and complexity-class containment. They have very different implications.
- Fresh 2025-2026 preprints need version/status lines, especially when later classical results revise the quantum-speedup story.

## Next Tranche

Recommended next cluster: remaining foundational models and topological/computation-universality papers, including one-way quantum computation, linear optics, Jones polynomial algorithms, area laws, and Viterbi/CSP/Ising notes.

## Completed Tranche 32 - Universality Models, Topological Polynomials, IQP Hardness, and Area Laws

| Note | Verdict | Review file |
|---|---:|---|
| [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/A One-Way Quantum Computer (Raussendorf-Briegel 2001) - Review]] |
| [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) - Review]] |
| [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) - Review]] |
| [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) - Review]] |
| [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) - Review]] |
| [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) - Review]] |
| [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) - Review]] |
| [[Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) - Review]] |
| [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/BQP and the Polynomial Hierarchy (Aaronson 2010) - Review]] |
| [[A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) - Review]] |
| [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) - Review]] |
| [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) - Review]] |
| [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]] | major factual issues | [[Review Notes/Paper Summaries/An Area Law for One-Dimensional Quantum Systems (Hastings 2007) - Review]] |
| [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) - Review]] |
| [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) - Review]] |

## Tranche 32 Pattern Notes

- Universality-model summaries should not compress resource patterns too far. MBQC CNOT gadgets, LOQC teleportation resources, and QFT scattering encodings all have important hidden overheads.
- Jones/Tutte/Potts notes must keep additive normalization scales visible. Formal polynomial-time additive approximation can be trivial on many instances.
- Sampling-hardness notes need three separate labels: multiplicative-error worst-case hardness, additive-error plus Stockmeyer, and average-case conjectures plus anticoncentration.
- Tensor-network and area-law notes must separate exact Schmidt rank, effective Schmidt rank, entropy, and MPS bond dimension. Bounded entropy is not the same as bounded exact rank.
- Resource-estimation notes age quickly. Keep solver names, hardware assumptions, decoder assumptions, and benchmark year next to numerical speedups.

## Next Tranche

Recommended next cluster: remaining quantum simulation, Hamiltonian/time-dynamics, QSP implementation, and quantum chemistry/catalysis notes, including sparse Markovian dynamics, QFT algorithms, power-law simulation, FeMoco/catalysis estimates, and real-time dynamics/QSP implementation papers.

## Completed Tranche 33 - Simulation, QSP Implementation, and Time-Dependent Dynamics

| Note | Verdict | Review file |
|---|---:|---|
| [[Efficient Simulation of Sparse Markovian Quantum Dynamics (Childs-Li 2017) — Paper Notes]] | needs rewrite | [[Review Notes/Paper Summaries/Efficient Simulation of Sparse Markovian Quantum Dynamics (Childs-Li 2017) - Review]] |
| [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) - Review]] |
| [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) - Review]] |
| [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) - Review]] |
| [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) - Review]] |
| [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) - Review]] |
| [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) - Review]] |
| [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) - Review]] |
| [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) - Review]] |
| [[Entanglement Accelerates Quantum Simulation (Zhao-Zhou-Childs 2025) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Entanglement Accelerates Quantum Simulation (Zhao-Zhou-Childs 2025) - Review]] |
| [[Quantum Divide and Conquer (Childs-Kothari-Kovacs-Deak-Sundaram-Wang 2025) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Divide and Conquer (Childs-Kothari-Kovacs-Deak-Sundaram-Wang 2025) - Review]] |
| [[Quantum Lower Bounds for Simulating Fluid Dynamics (Ameri-Carolan-Childs-Krovi 2026) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum Lower Bounds for Simulating Fluid Dynamics (Ameri-Carolan-Childs-Krovi 2026) - Review]] |
| [[Locality and Digital Quantum Simulation of Power-Law Interactions (Tran-Guo-Su-Childs-Gorshkov 2019) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Locality and Digital Quantum Simulation of Power-Law Interactions (Tran-Guo-Su-Childs-Gorshkov 2019) - Review]] |
| [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) - Review]] |
| [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) - Review]] |
| [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) - Review]] |

## Tranche 33 Pattern Notes

- Open-system simulation summaries must distinguish a vectorized proof representation from the algorithm's physical output. A Lindbladian simulator implements a channel; it does not usually output a pure vectorized density matrix.
- QSP/QSVT notes are strongest when they separate three layers: polynomial approximation degree, numerical phase-factor synthesis, and coherent versus randomized/channel implementation.
- Fresh 2025-2026 papers need version and publication metadata checked. In this tranche: Martyn-Rall is npj Quantum Information 11, 47 (2025); SOSSA is a 2025 arXiv paper later appearing as PRL 136, 110601 (2026); the fluid-dynamics lower-bound note had two wrong author names.
- Long-range/locality simulation notes should quote the actual alpha regimes from the source. Replacing HHKL/locality-decomposition bounds by a simplified first-order Trotter table can change the theorem.
- "Near-optimal" and "tight" require a named lower-bound model. Query complexity, copy complexity, diamond-norm channel error, and physical gate counts are different resources.

## Next Tranche

Recommended next cluster: remaining quantum chemistry, catalysis, and electronic-structure application notes, followed by tomography/shadow/learning and information-theoretic papers.

## Completed Tranche 34 - Many-Body, TQFT, Hardware, and Catalysis Simulation Applications

| Note | Verdict | Review file |
|---|---:|---|
| [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) - Review]] |
| [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) - Review]] |
| [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) - Review]] |
| [[Quantum-Circuit Design for Efficient Simulations of Many-Body Quantum Dynamics (Raeisi-Wiebe-Sanders 2012) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Quantum-Circuit Design for Efficient Simulations of Many-Body Quantum Dynamics (Raeisi-Wiebe-Sanders 2012) - Review]] |
| [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) - Review]] |
| [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) - Review]] |
| [[Digital Quantum Simulation of Minimal AdS-CFT (García-Álvarez, Egusquiza, Lamata, del Campo, Sonner, Solano 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Digital Quantum Simulation of Minimal AdS-CFT (García-Álvarez, Egusquiza, Lamata, del Campo, Sonner, Solano 2017) - Review]] |
| [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) - Review]] |
| [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) - Review]] |
| [[Quantum Computations with Cold Trapped Ions (Cirac-Zoller 1995) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computations with Cold Trapped Ions (Cirac-Zoller 1995) - Review]] |
| [[Observation of Separated Dynamics of Charge and Spin in the Fermi-Hubbard Model (Arute et al. 2020) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Observation of Separated Dynamics of Charge and Spin in the Fermi-Hubbard Model (Arute et al. 2020) - Review]] |
| [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes]] | needs rewrite / retitling | [[Review Notes/Paper Summaries/Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) - Review]] |

## Tranche 34 Pattern Notes

- Tensor-network notes should separate representability, contraction cost, and variational optimization guarantees.
- Integrable/free-fermion circuit notes must not generalize from free-fermion diagonalization to all integrable or Bethe-ansatz-solvable models.
- Resource-estimation notes must keep active space, target accuracy, oracle or block-encoding normalization, and hardware model adjacent to every headline number.
- Experimental/NISQ notes need exact qubit and gate counts, and should distinguish calibration/classically-solvable regimes from the central interacting-physics claim.
- Metadata/title mismatches need explicit triage. The Knill-Ortiz-Somma file content is expectation-value measurement, not classical annealing simulation.

## Next Tranche

Recommended next cluster: tomography, shadow, learning, and information-theoretic notes.

## Completed Tranche 35 - Tomography, Shadows, Learning, Entanglement, and Quantum Information

| Note | Verdict | Review file |
|---|---:|---|
| [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) - Review]] |
| [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Shadow Tomography of Quantum States (Aaronson 2018) - Review]] |
| [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) - Review]] |
| [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) - Review]] |
| [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) - Review]] |
| [[Predicting quantum channels over general product distributions (Chen-de Dios Pont-Hsieh-Huang-Lange-Li 2024) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Predicting quantum channels over general product distributions (Chen-de Dios Pont-Hsieh-Huang-Lange-Li 2024) - Review]] |
| [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) - Review]] |
| [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/The Learnability of Quantum States (Aaronson 2006) - Review]] |
| [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) - Review]] |
| [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Power of One Bit of Quantum Information (Knill-Laflamme 1998) - Review]] |
| [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) - Review]] |
| [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) - Review]] |
| [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) - Review]] |
| [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) - Review]] |
| [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) - Review]] |
| [[Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) - Review]] |
| [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]] | needs rewrite | [[Review Notes/Paper Summaries/Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) - Review]] |
| [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) - Review]] |

## Tranche 35 Pattern Notes

- Shadow/tomography notes must keep measurement model, copy complexity, and classical runtime separate. "Shadow" can mean collective shadow tomography, single-copy classical shadows, or two-copy Pauli shadow tomography.
- Learning-theory notes need their distributional assumptions next to every efficiency claim. Average-case prediction from sampled data is not the same as worst-case simulation or full reconstruction.
- QML advantage claims need data-access, kernel-conditioning, and natural-versus-engineered dataset caveats in the headline, not only in a later limitations section.
- Quantum-information protocols should state the resource convention before the rate: free classical communication, quantum communication, entanglement consumption, or entanglement generation.
- Entanglement-resource summaries need the exact resource notion. `p`-blocked states, bounded Schmidt rank, stabilizer states, and high-purity subsystems define different simulation/trainability boundaries.

## Next Tranche

Recommended next cluster: fault tolerance, topological computation, stabilizer simulation, linear optics hardness, interactive proofs, and other complexity-theory foundations.

## Completed Tranche 36 - Fault Tolerance, Topological Computation, Stabilizers, Sampling Hardness, and Complexity Foundations

| Note | Verdict | Review file |
|---|---:|---|
| [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) - Review]] |
| [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) - Review]] |
| [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) - Review]] |
| [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computing with Realistically Noisy Devices (Knill 2005) - Review]] |
| [[Theory of Quantum Error-Correcting Codes (Knill-Laflamme 1997) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Theory of Quantum Error-Correcting Codes (Knill-Laflamme 1997) - Review]] |
| [[Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) - Review]] |
| [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) - Review]] |
| [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) - Review]] |
| [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes]] | major issues | [[Review Notes/Paper Summaries/PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) - Review]] |
| [[QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) - Review]] |
| [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Arthur-Merlin Games (Marriott-Watrous 2005) - Review]] |
| [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) - Review]] |
| [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) - Review]] |
| [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) - Review]] |
| [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) - Review]] |
| [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) - Review]] |

## Tranche 36 Pattern Notes

- Fault-tolerance notes should separate rigorous threshold theorems, numerical evidence, and physical topological protection. These are different standards of support.
- Topological-computation summaries need stable convention boxes for level/root-of-unity parameters; otherwise "k = 4", "CS_5", Ising, Fibonacci, and Jones roots get conflated.
- Complexity notes must track message count, round count, and public-coin conventions. `QIP(2)`, "two-round", and "three-message" are not interchangeable.
- Sampling-hardness notes should display the implication chain and assumptions: exact versus approximate sampling, Stockmeyer extraction, anti-concentration, average-case hardness, and noise robustness.
- Query/streaming algorithm summaries should keep access model, time, query count, memory, and pass count adjacent to every result.

## Next Tranche

Recommended next cluster: TDA, PDE/linear systems, partition functions, state preparation, calibration, algorithmic cooling, and resource-estimation notes.

## Completed Tranche 37 - TDA, PDE/Linear Systems, Partition Functions, State Preparation, Calibration, Cooling, and Resource Estimation

| Note | Verdict | Review file |
|---|---:|---|
| [[Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Sáenz-de-Cabezón 2013) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Sáenz-de-Cabezón 2013) - Review]] |
| [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) - Review]] |
| [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) - Review]] |
| [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) - Review]] |
| [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) - Review]] |
| [[Quantum Algorithms for Systems of Linear Equations Inspired by Adiabatic Quantum Computing (Subaşı-Somma-Orsucci 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Systems of Linear Equations Inspired by Adiabatic Quantum Computing (Subaşı-Somma-Orsucci 2018) - Review]] |
| [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574)]] | minor-to-major issues | [[Review Notes/Paper Summaries/Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) - Review]] |
| [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) - Review]] |
| [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) - Review]] |
| [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) - Review]] |
| [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) - Review]] |
| [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) - Review]] |
| [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) - Review]] |
| [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) - Review]] |
| [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) - Review]] |
| [[Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012) - Review]] |
| [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) - Review]] |
| [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) - Review]] |
| [[Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) - Review]] |
| [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) - Review]] |
| [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) - Review]] |
| [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]] | minor-to-major issues | [[Review Notes/Paper Summaries/Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) - Review]] |
| [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) - Review]] |
| [[SOSSA — Sum-of-Squares Spectral Amplification]] | minor issues / duplicate coverage | [[Review Notes/Paper Summaries/SOSSA — Sum-of-Squares Spectral Amplification - Review]] |
| [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) - Review]] |
| [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]] | minor issues | [[Review Notes/Paper Summaries/Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) - Review]] |

## Tranche 37 Pattern Notes

- TDA notes should state the exact task before claiming advantage: exact Betti numbers, normalized additive Betti estimates, persistent Betti ranks, harmonic-persistence decision problems, and barcode recovery are different computational problems.
- Linear-system, FEM, Macaulay, and PDE notes need condition number, input-state preparation, sparse/block-encoding access, and output/readout models next to the headline runtime.
- Partition-function and Monte Carlo notes should separate query speedups from end-to-end algorithms. State preparation, arithmetic, Markov-chain mixing, and variance reduction are often the real bottlenecks.
- Resource-estimation and state-preparation notes should keep cost models separate: T count, Toffoli count, Clifford count, routing depth, dirty/clean ancillae, and physical surface-code costs are not interchangeable.
- Recent software/preprint notes need version and metadata lines. Qualtran, Quantum CORDIC, SOSSA, and the 2025 block-encoding preprint are especially likely to drift.
- Alias or pointer notes under `Paper Summaries` should be labeled explicitly. `SOSSA — Sum-of-Squares Spectral Amplification` duplicates the full structured SOSSA paper note and should not be mistaken for an independent unreviewed paper.

## Paper Summary Coverage Status

All Markdown paper-summary notes have companion reviews. The remaining unmatched files under `Paper Summaries/` are Excalidraw drawings, not paper notes.
