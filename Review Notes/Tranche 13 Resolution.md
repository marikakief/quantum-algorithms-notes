# Tranche 13 Resolution

Scope: modular arithmetic, multiplication, Shor-resource arithmetic, and Clifford+T synthesis notes reviewed in Tranche 13.

## Paper notes

- `Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017)`: implemented. Clarified that dirty ancillae are restored, not consumed. Qualified QFT-comparison rows so the extra logarithms are tied to fault-tolerant rotation-synthesis precision. Corrected the later Gidney-Ekerå comparison to the abstract formula value at RSA-2048, about `2.6e9` Toffoli+T/2 operations, and added a later-landscape caveat separating logical qubits, Toffoli/T count, T depth, and physical area-time.

- `Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017)`: implemented. Replaced the inconsistent space-exponent formula with the correct `1 + (log_2 3 - 1)/(2 - log_3 2)` expression, labeled the `n=400` count as the forward-multiplier comparison before extra outer uncomputation, and recomputed the `n=2048` exponent-only space ratio as about `3.3x`. The existing caveat already distinguishes general quantum multiplication from Shor-style multiplication by classical constants.

- `Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018)`: implemented. Reworded the piecewise-polynomial section to coherent selected-polynomial evaluation via label-controlled coefficient loads, added fixed-point sign/binary-point assumptions next to the formulas, moved the compute-only warning before Table II counts, and added an arithmetic-vs-QROM interpolation caveat.

- `Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019)`: implemented. Put all assumptions behind the `O(log log n)` depth claim in one sentence, clarified that runway values are part of the encoded representation rather than classical carry registers, specified that `k` is the number of additions in one encoded computation with union-bound style composition, tied practical volume claims to the paper's surface-code/factory assumptions, and contrasted the error model with optimistic circuits.

- `Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013)`: implemented. Corrected the small-angle success probability so the leading failure term is quadratic in `theta`, marked the composed-gearbox T-count formula as schematic rather than a theorem statement, replaced the overstrong ancilla-assistance wording with model-specific empirical evidence, and added retry-budget/tail-bound language for RUS resource estimates.

- `Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014)`: implemented. Added a model box specifying deterministic ancilla-free Clifford+T, operator norm, and fixed/up-to-phase variants. Repeated the prime-distribution-hypothesis condition in the typical/without-factoring result and comparison row.

- `The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005)`: implemented. Replaced the too-specific pre-SK overhead strawman with polynomial-in-`1/epsilon` overhead, separated the information-theoretic lower bound from Harrow-Recht-Chuang's near-optimal sequence-length result, added the SU(2)-vs-SU(d) constants caveat for balanced commutators, and added a Ross-Selinger comparison for modern Clifford+T rotations.

- `Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019)`: implemented. Scoped the `n`-Toffoli claim to the comparator per oracle call, tied the 286-375x comparison to the paper's arithmetic baseline, corrected the reference-register step to interference/unprepare rather than measurement, moved oracle-cost caveats closer to the headline claim, and added that amplitude amplification still depends on the success amplitude/norm.
